# Completar la compra

Acabo de comprar otro producto increíble en nuestra tienda online, y cuando terminé de pagar, LemonSqueezy me dio un mensaje de éxito:

> ¡Gracias por tu pedido!

Es breve, dulce y directo, pero ¿hay alguna forma de personalizarlo si lo necesitamos? Sí Y es específico para cada producto.

En el panel de control, haz clic en "Tienda"... "Productos"... "Elegir un producto", y en la sección "Modalidad de confirmación" de aquí abajo, busca los campos "Título" y "Mensaje". Aquí tenemos un texto por defecto, el mismo que vimos antes en el mensaje de confirmación.

De momento estoy bastante contento con el texto por defecto, aunque le vendrían bien unos cuantos signos de exclamación más porque estoy muy emocionado, así que lo dejaré como está por ahora. Si decides cambiar este texto, recuerda que los cambios que hagas aquí sólo se aplicarán a este producto concreto, así que si quieres que todos tus productos reflejen tus cambios, tendrás que personalizarlos uno a uno.

Lo mismo ocurre con este botón. Aquí podemos cambiar su texto y su enlace. El enlace por defecto parece que va a una página de mis pedidos. Si hacemos clic en él... ¡sí! Estamos en la página de pedidos de LemonSqueezy.

## Borrar el carrito tras la compra

En el sitio web... el producto que acabamos de comprar sigue en el carrito. Tenemos que asegurarnos de que el carrito se borra después de realizar una compra. Para ello, en `OrderController`, crea una acción especial. La llamaremos `success()`. A continuación, registra la ruta en`#[Route('/checkout/success', name: 'app_order_success')]`. Esto redirigirá a los clientes a esta ruta.

Ahora, para evitar el acceso directo a esta página, vamos a utilizar un pequeño truco. Inyecta `Request $request` y, dentro, añade`$referer = $request->headers->get('referer')`. A continuación, crea una variable -`$lsStoreUrl` - y establécela en la URL de la tienda. Para ello, ve al panel de control, abre el escaparate, copia la URL y pégala en nuestro código`https://squeeze-the-day.lemonsqueezy.com'`.

A continuación, añade `if (!str_starts_with($referer, $lsStoreUrl))`. Así, si esto es cierto, y alguien abre esta URL directamente, podemos simplemente redirigirle a la página de inicio con `return $this->redirectToRoute('app_homepage')`. Inyecta`ShoppingCart $cart` y, a continuación, continúa con `if ($cart->isEmpty())`. Por último, vuelve de nuevo a la página de inicio con`return $this->redirectToRoute('app_homepage')`. Si no, vacía el carrito con `$cart->clear()`.

Podríamos crear una página de éxito independiente con algunos detalles si quisiéramos, pero por ahora lo haremos sencillo y sólo añadiremos un mensaje flash -`$this->addFlash('success', 'Thanks for your order!')` - y`return $this->redirectToRoute('app_homepage')`.

Vale, ahora tenemos que añadir esta URL al campo "Enlace del botón" de todos y cada uno de los productos. Un fastidio. Tiene que haber una forma más fácil de hacerlo, ¿verdad? Afortunadamente, sí: con una opción de la API.

En los documentos de la API, busca "Crear una compra". En `product_options`, mira esto`redirect_url`:

> Una URL personalizada a la que redirigir después de una compra satisfactoria.

¡Eso es lo que estamos buscando!

En nuestro código, abre el método `createLsCheckoutUrl()`, y debajo, añade:`$attributes['product_options']['redirect_url'] = $this->generateUrl('app_order_success', [], UrlGeneratorInterface::ABSOLUTE_URL)`.

Bien, vuelve a pasar por caja. Introduce los datos de la tarjeta y la dirección de facturación, haz clic en el botón "Realizar pedido" y espera a que aparezca el modal de confirmación. ¡Ya está! Si hacemos clic en "Continuar"... ¡sí! Vemos el mensaje flash de "realizado con éxito" - "¡Gracias por tu pedido!" - y el carrito ya está completamente vacío. ¡Woohoo!

Siguiente: Antes de hacer más peticiones a la API, separemos la lógica empresarial de LemonSqueezy del controlador y centralicémosla en un servicio independiente.
