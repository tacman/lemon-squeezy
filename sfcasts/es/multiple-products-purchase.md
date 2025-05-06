# Compra de varios productos

La compra de un solo producto es impresionante. Incluso podemos especificar la cantidad y se reflejará en la página de pago de LemonSqueezy. Pero, ¿qué pasa si añadimos un producto más al carrito? Un producto diferente. Sí, hay un problema. Hemos pulsado el botón de pago con dos sabores de limonada en nuestro carrito, pero sólo un sabor -el primero de nuestro carrito- aparece en la página de pago. ¿Cómo podemos solucionarlo? Bueno... hay un pequeño problema.

Actualmente, LemonSqueezy limita el número de artículos que podemos comprar a sólo uno, lo que es un fastidio. Y aunque su hoja de ruta sugiere que esto puede cambiar en el futuro, eso no nos ayuda mucho ahora mismo. ¿Qué podemos hacer? Ser creativos, ¡por supuesto!

Si echamos un vistazo a los documentos de la API, LemonSqueezy nos permite fijar nuestro propio precio. Así que probemos algo... para la ciencia. En `OrderController.php`, dentro de`createLsCheckoutUrl()`, añade `if (count($products) === 1)`. Aquí arriba, corta las tripas de la variable `$attributes`, y ponla en una matriz vacía. En nuestra nueva declaración `if`, escribe `$attributes`... pega... y... ¡listo!

[[[ code('f0e9c119e4') ]]]

Bien, ahora tenemos que cambiar `quantity`. Copia `$cart->getProductQuantity()`, elimina esa línea y pégala aquí abajo. Debajo de eso, añade `else`, y dentro, escribe `$attributes['custom_price'] = $cart->getTotal()` y`$attributes['product_options']` en una matriz donde`'name' => 'E-lemonades'`, para que el nombre sea más universal para nuestros usuarios.

Esto tiene buena pinta, pero creo que podemos mejorarlo aún más. Entre nuestro`$attributes` aquí, escribe `$description = ''`. Debajo, iteraremos sobre nuestros productos con `foreach ($products as $product)`. Dentro, pon`$description .= $product->getName() . 'for $' .
number_format($product->getPrice()/100, 2) . ' x ' .
$cart->getProductQuantity($product)`. Añade un `. '<br>'` al final. No olvides terminar esto con un `;`. Por último, en `product_options`, vamos a utilizar esta variable con`'description' => $description,`. Muy bien.

[[[ code('f6bdda62cb') ]]]

Muy bien, ¡vamos a probarlo! Ve de nuevo a la página del carrito y vuelve a cargarla, por si acaso. Haz clic en "Pagar con LemonSqueezy" y... ¡funciona! Podemos ver el nombre del producto "E-lemonades" por 8,97 $, y debajo, una descripción con la lista de productos, la cantidad y el precio. Puede que no sea lo ideal, ¡pero funciona! No queremos obligar a nuestros clientes a pasar por todo el proceso de pago para cada producto que quieran comprar. ¡Piensa en todos los beneficios que perderíamos!

Incluso podríamos ir un paso más allá y cambiar cosas como la imagen que se muestra en la página de pago, lo que también es posible a través de la API. O mejor aún, podríamos crear un producto de variedad "E-lemonade" en el panel de control de LemonSqueezy y utilizar ese `variantId` para que sea aún más obvio que estamos comprando una mezcla de productos e-lemonade en lugar de un único producto específico.

El problema es que, aunque cambiemos el nombre y la descripción del producto en la página de pago de LemonSqueezy, LemonSqueezy seguirá utilizando el nombre y la imagen originales en correos electrónicos y recibos. Eso puede cambiar algún día, pero de momento, es con lo que estamos trabajando.

Si rellenamos unos datos de facturación falsos, otros campos obligatorios en la página de pago y hacemos clic en el botón "Pagar"... recibimos este mensaje de confirmación de pedido. Si comprobamos nuestro correo electrónico... sí, sólo vemos la información del primer producto. Faltan nuestro nombre personalizado y la descripción. Lo mismo ocurre con la página donde vemos nuestro pedido. Así que esto no es perfecto...

A continuación: Pulamos aún más nuestro proceso de pago con algunas operaciones posteriores al pago.
