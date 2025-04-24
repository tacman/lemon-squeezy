# Datos dinámicos

¡Muy bien! ¡Hemos hecho nuestra primera petición a la API! Esto genera una URL de pago única para nuestro cliente y abre la página de pago de LemonSqueezy para que pueda comprar el producto. Pero cuando configuramos esto, codificamos muchas cosas. ¡Es hora de hacerlo dinámico!

## Utilizar datos dinámicos en el objeto de pago

Empecemos con el ID de la tienda. Es único para nuestros entornos de prueba y real, así que tiene sentido establecerlo como una variable de entorno. Abre el archivo`.env` y, debajo de `LEMON_SQUEEZY_API_KEY`, escribe`LEMON_SQUEEZY_STORE_ID=` y establece ese valor como el ID de la tienda. Puedes encontrarlo en `OrderController.php`.

Ahora, en `config/services.yaml`, debajo de `parameters`, añade uno nuevo -`env(LEMON_SQUEEZY_STORE_ID)` - y establécelo como`%env(LEMON_SQUEEZY_STORE_ID)%’`. De vuelta en nuestro controlador, sustituye el valor ID por `$this->getParameter('env(LEMON_SQUEEZY_STORE_ID)')`.

## Almacenar ID de variantes en la base de datos

Para el ID de variante, vamos a crear un nuevo campo en la entidad `Product`. En tu terminal, ejecuta:

```terminal
bin/console make:entity
```

Actualiza la entidad `Product`, y llama al nuevo campo `lsVariantId`. Será un `string`, con una longitud de campo por defecto, y hagámoslo "anulable". Pulsa`Enter` para terminar y luego dirígete a `src/Entity/Product.php` para ver nuestros cambios. Si nos desplazamos hacia abajo... ¡ah, ahí está! Probablemente también deberíamos hacer esto`unique`. ¡Tiene buena pinta! Aquí abajo, podemos ver que también ha creado un getter y un setter. ¡Qué práctico!

Ahora, de vuelta en nuestro terminal, crea una migración con:

```terminal
bin/console make:migration
```

¡Vamos a comprobarlo! De vuelta en `migrations/`... aquí abajo... parece que se ha añadido una nueva columna. ¡Qué bien! Aquí arriba, podemos añadir una descripción:

> Añadir una columna para almacenar el ID de variante LS en Producto.

De vuelta a nuestro terminal, ejecuta la migración:

```terminal
bin/console doctrine:migrations:migrate
```

Ahora, en `src/DataFixtures/AppFixtures.php`, establece nuestro nuevo campo en el ID de variante del panel de control de LemonSqueezy. Puedes encontrarlo haciendo clic en "Tienda", " Productos", los tres puntos al final de nuestro producto aquí, y seleccionando "Copiar ID de variante". Pégalo y... ¡tachán! ¡El primero está hecho! Pero, ¿qué clase de puesto de limonada de diseño seríamos si sólo tuviéramos un tipo de limonada para elegir? ¡Añadamos más en función de nuestros accesorios!

Copia esta descripción y, de nuevo en nuestro panel de control, crea un nuevo producto. A éste lo llamaremos "Limonada electrónica de sandía" y pégalo. Establece el precio en "1,99 $", añade una imagen y "Publica el producto". Copia el ID de variante de nuestro nuevo producto y añádelo a nuestro producto en `AppFixtures.php`. ¡Estupendo! Hagamos lo mismo con los cuatro productos siguientes. Lo aceleraré un poco.

¡Listo! Ahora vamos a recargar los accesorios con:

```terminal
bin/console doctrine:fixtures:load
```

De vuelta en el controlador, dentro de `createLsCheckoutUrl()`, recupera los productos del carrito de la compra con `$products = $cart->getProducts()`. Y estableceremos`$variantId` en `$products[0]->getLsVariantId()` por ahora. Por último, debajo de`$response`, establece el ID de variante en `$variantId`.

## Establece la cantidad correcta

Bien, ¡intentemos pasar por caja! Vuelve a la página de inicio y elige esta vez un nuevo producto. Establece la cantidad en "2", añádelo al carrito y haz clic en el botón de pago. ¡Qué bien! Estamos en la página de pago, y éste es el producto correcto pero... no la cantidad correcta.

Para solucionarlo, de vuelta en nuestro código, debajo de `type`, añade `attributes`...`checkout_data`... `variant_quantities`... otra matriz vacía, y dentro de ésta, di `variant_id => $variantId` y `quantity => $quantity`. Arriba, debajo de `variantId`, añade `$quantity = $cart->getProductQuantity()` con`$prodcuts[0]` como argumento. Si vamos a la página del carrito y volvemos a pulsar el botón "pasar por caja"... ¡sí! ¡Tenemos el producto correcto y la cantidad correcta!

## Rellenar previamente los datos del usuario

Si me desplazo hacia arriba... hm... ¿de dónde viene este correo electrónico? Soy un propietario autenticado de una tienda LemonSqueezy, así que esto está pre-rellenado para mí. Pero, ¿y si intento pagar como cliente en modo incógnito? Abro una nueva pestaña en modo incógnito, me conecto de nuevo con `lemon@example.com` como correo electrónico y `lemonpass` como contraseña, elijo un producto, la cantidad y lo añado al carrito, hago clic en "pagar" y... ¡ajá! ¡Los datos de usuario están vacíos! El usuario puede rellenarlos él mismo, así que no es un gran problema, pero apuesto a que podemos ahorrarle algo de tiempo y rellenarlos previamente desde nuestra aplicación, puesto que ya compartió su nombre y correo electrónico cuando se autenticó. Hagámoslo

En `OrderController::checkout()`, añadamos un nuevo argumento -`#[CurrentUser] ?User $user` - y más abajo, pasemos `$user` a`createLsCheckoutUrl()`. Aquí abajo en nuestra función, añade un tercer argumento:`?User $user`. Debajo de `$quantity`, añade `$attributes =` y ajústalo a la matriz de atributos de las opciones de la petición. Podemos copiar y pegar esto para hacerlo más fácil.

Lo estableceremos en `$attributes`, y aquí arriba, añade `if ($user)`. Dentro, di`$attributes['checkout_data']['email'] = $user->getEmail()`. Y abajo,`$attributes['checkout_data']['name'] = $user->getFirstName()`. Perfecto Probemos de nuevo a realizar la compra en modo incógnito. Vuelve a la página de inicio, haz clic en el carrito donde ya tenemos dos artículos esperando, haz clic en el botón de pago y... ¡los datos del usuario ya están rellenados! ¡Fantástico!

Hasta ahora, sólo hemos probado a comprar un sabor de limonada cada vez. Intentemos comprar más de un tipo a continuación.
