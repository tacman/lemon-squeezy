# Petición de API de pago

En nuestro sitio web, aquí abajo, hacemos clic en un producto para abrir su página y añadirlo al carrito. Si hacemos clic en el botón "Realizar pedido"... no ocurre nada. ¡Vamos a arreglarlo!

En el capítulo anterior, vimos cómo generar una URL de pago única para cada producto LemonSqueezy en el panel de control y compartirla con el cliente. ¿Y adivina qué? Podemos hacer lo mismo con la ruta API de pago

## Crear una URL de pago

Ya tenemos un cliente HTTP configurado, así que vamos a hacer nuestra primera petición a la API de LemonSqueezy. Para encontrar la URL, abre los documentos de la API, desplázate hacia abajo y haz clic en [Create a checkout](https://docs.lemonsqueezy.com/api/checkouts/create-checkout). Esto nos muestra la URL específica del punto final, el método que tenemos que utilizar y también tiene un montón de opciones de configuración. ¡Incluso hay un ejemplo con una respuesta JSON!

Si nos desplazamos por el código... justo aquí... ahí está: ¡la clave `url`! Se encuentra dentro de `data`, `attributes`. Cuando nuestros clientes pulsen el botón "Pagar", queremos redirigirles a esta URL para completar el pago ¡No hay problema!

De vuelta a nuestro código, ya tenemos `OrderController.php`, que contiene nuestros métodos relacionados con el carrito. Creemos uno nuevo llamado `checkout()`... y convirtámoslo en una ruta con `#[Route('/checkout', name: 'app_order_checkout')]`. ¡Fantástico!

[[[ code('07657fc1c4') ]]]

Ahora, abre `cart.html.twig`... y establece el "Pago con LemonSqueezy" `href` en `path('app_order_checkout')`. 

[[[ code('6576526b97') ]]]

Gracias a esto, cuando un cliente haga clic en el botón "Pagar", llegará a esta ruta. Nuestra siguiente tarea es generar la URL de pago de LemonSqueezy a través de la API y redirigir a nuestros clientes para que puedan completar su compra.

Para ello, inyecta `HttpClientInterface $lsClient` y `ShoppingCart $cart`... y traslademos la lógica de negocio real de la llamada a la API a un método aparte por comodidad. Aquí abajo, escribe`$lsCheckoutUrl = $this->createLsCheckoutUrl($lsClient, $cart);` y, por último,`return $this->redirect($lsCheckoutUrl);`. ¡Se ve bien!

[[[ code('35d6030e8d') ]]]

Para crear un nuevo método, mantén pulsadas las teclas "Opción" + "Intro" en un Mac para abrir el menú y elige "Añadir método" para que PhpStorm lo haga por nosotros. ¡Qué práctico! Esto devolverá un`string`... y dentro, hagamos una comprobación de cordura:

`if ($cart->isEmpty())`
`throw new \LogicException('Nothing to checkout!');`

## Haz una petición a la API de LS

Abajo, escribe`$response = $lsClient->request(Request::METHOD_POST, 'checkouts', []);`... y dentro, `'json' => ['data' => ['type' => 'checkouts']]`. 

[[[ code('8a60288e0f') ]]]

Podemos dejar el resto de opciones vacías por ahora. Los documentos de la API de LemonSqueezy no aclaran realmente qué opción es necesaria, así que tendremos que averiguarlo por nuestra cuenta.

Aquí abajo, como tenemos una respuesta JSON, tenemos que convertirla en una matriz con`$lsCheckout = $response->toArray();`. Luego, `return`... y a partir del ejemplo de respuesta que vimos en los documentos, podemos leer la URL con`$lsCheckout['data']['attributes']['url'];`. ¡Estupendo!

[[[ code('af97f0bb3e') ]]]

¡Muy bien! ¡Hora de probar! Volvemos a nuestro sitio, actualizamos, hacemos clic en el botón "Comprobación", y... ¡un error!

> URL no válida: falta el esquema en "checkouts". ¿Has olvidado añadir
> ¿"http(s)://"?

Hm... parece que ignoró nuestra URL base del cliente de ámbito. Sospecho que en su lugar ha inyectado el cliente vacío por defecto. ¡No te preocupes! ¡Estamos en ello!

En tu terminal, ejecuta

```terminal
bin/console debug:autowiring HttpClientInterface
```

para ver los servicios relacionados. Ah, ja... para inyectar el cliente de LemonSqueezy, necesitamos utilizar autocableado con nombre y utilizar el nombre exacto de la variable:`$lemonSqueezyClient`. Lo acortamos a `$lsClient` en el código. ¡Vamos a arreglarlo!

Cámbialo por `$lemonSqueezyClient`, y se solucionará nuestro problema, pero... espera... Me gusta la versión más corta que estábamos utilizando. ¿Hay alguna forma de seguir utilizándola sin romper nuestra integración? ¡Por supuesto! En lugar de limitarnos a cambiarle el nombre, aprovechemos el atributo PHP `#[Target]` para enlazarlo con el servicio correcto. Sobre el argumento, añade `#[Target('lemonSqueezyClient')]`.

[[[ code('cb6b4d1923') ]]]

Si nos dirigimos e intentamos todo esto de nuevo... ugh... genial... otro error. Quiero decir... un error diferente:

> HTTP/2 422 devuelto para "https://api.lemonsqueezy.com/v1/checkouts".

Hm... un código de estado 422 nos dice que el servidor no ha podido procesar la petición porque contiene datos no válidos, pero la excepción no nos da mucha información. Probemos a volcar el contenido de la respuesta.

Aquí detrás, antes de la devolución, añade `dd($response->getContent());`. 

[[[ code('a5bcbe0512') ]]]

Si lo intentamos de nuevo... hm, obtenemos el mismo error. Ah... parece que tenemos que pasar`false` a `getContent()` para depurar la respuesta sin lanzar una excepción. Añade `false` aquí... actualiza una vez más, y... ¡bien! En los detalles de aquí, podemos ver que `variant.id` es necesario, así como `store.id`. Incluso nos indica las rutas específicas que necesitamos:`data/relationships/store/data/id` y `data/relationships/variant/data/id`. Podemos seguir esa ruta en los documentos de la API: `data`... `relationships`... `stores`...`data`... `id`... y así sucesivamente. Sigue adelante y añade esto a nuestro código -`relationships`, `stores`, `data`, `'type' => 'stores'`, y `'id' =>`.

Para encontrar el ID real de la tienda que necesitamos, ve al panel de control de LemonSqueezy y haz clic en "Configuración", [Tiendas](https://app.lemonsqueezy.com/settings/stores). Copia el id aquí y pégalo en nuestro código.

El `variant` debe seguir una ruta similar a `store`, así que escribiremos `variant`...`data`... `'type' => 'variants'`... y para `id`, vamos a codificar uno del producto que creamos en el primer capítulo. 

[[[ code('bb215b71e1') ]]]

Abre el panel de control, ve a "Tienda", "Productos", y a la derecha, haz clic en los tres puntos de aquí para abrir un menú. Aquí abajo, haz clic en "Copiar ID de variante" y pégalo en nuestro código. ¡Listo!

De vuelta en la página de pago, actualiza y... otro error. Este nos dice que el ID de variante debe ser una cadena. Si echamos un vistazo a nuestro código... ¡sí! ¡Es fácil de arreglar! Actualmente es un número entero, pero si hacemos esto... ¡tachán!

[[[ code('26290306fb') ]]]

¡Ahora es una cadena! Haz lo mismo para `variant`, luego vuelve atrás y actualiza de nuevo. Tenemos una respuesta JSON y, si ampliamos esto y hacemos una búsqueda rápida... ¡parece que tiene la URL que necesitamos! ¡Qué bien! Vuelve atrás y comenta el `dd()`, actualiza la página de nuevo y... ¡sí! Estamos en la página de pago de LemonSqueezy, ¡listos para comprar una deliciosa limonada electrónica!

A continuación: Hagamos que nuestros datos codificados sean dinámicos.
