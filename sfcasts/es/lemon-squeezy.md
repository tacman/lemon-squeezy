# Conoce a LemonSqueezy - Tu comerciante de confianza.

¡Hola y bienvenidos a otro tutorial de comercio electrónico! No quiero vender esto demasiado fuerte, ¡pero creo que este tutorial es fantástico! Hay un montón de procesadores de pagos disponibles, lo que significa que vender productos online es fácil y emocionante... hasta que te das cuenta de que gestionar los impuestos, y el cumplimiento de las normativas es... bueno... no es emocionante.

¿Podemos simplificar la forma de gestionar toda esa información? ¡Claro que sí! Para ello, necesitamos un procesador de pagos que sea un Comerciante Registrado, o "MdR". "¿Qué es eso? Es como cualquier procesador de pagos, pero con superpoderes. "Merchant of Record" significa que se encarga de todas las cuestiones legales y financieras complicadas, como los impuestos, por ti. Se acabaron los quebraderos de cabeza por el IVA y las pesadillas por el cumplimiento de las normativas.

Te mostraremos cómo configurarlo con Lemon Squeezy, pero lo que vamos a explicar se puede aplicar a otros servicios como Paddle o Polar. El servicio que elijas depende de ti: 
vamos a utilizar LemonSqueezy, pero no tenemos un favorito.

Bien, ¡lo primero es lo primero! Tenemos que registrarnos para poder empezar a vender productos. Haz clic en "Empezar" y añade tu nombre, dirección de correo electrónico y una contraseña súper segura, ya sabes... porque estamos tratando con cosas de pago. También puedes configurar 2FA más adelante para tener aún más seguridad. Recuerda utilizar tu correo electrónico real porque tendrás que confirmarlo antes de poder empezar. También enviaremos algunos correos de prueba que querrás ver.

## Un recorrido rápido por el panel de control

Yo ya he registrado el mío, así que sólo tengo que volver e iniciar sesión con mis credenciales. Y... ¡bienvenido a "Exprime el Día", mi puesto de limonada de diseñador digital! Ya tengo algunos datos en el gráfico de mi panel de control que adquirí mientras preparaba este tutorial. El tuyo debería estar completamente vacío.

Al registrarte, aparecerá una página de configuración que contiene varios pasos que tendrás que completar para poner en marcha tu tienda, pero de eso ya nos preocuparemos más adelante. Por ahora, podemos empezar a trabajar en nuestra integración en modo de prueba. Podemos saber que estamos en "modo de prueba" marcando este pequeño interruptor aquí abajo en la barra lateral. Eso significa que podemos utilizar números de tarjeta de prueba para simular pagos sin gastar dinero real. Eso va a ser superútil porque estoy a punto de gastar un montón de dinero mientras desarrollamos. Mi cartera falsa ya está llorando. En cuanto termines los pasos de configuración y actives tu tienda, podrás cambiar entre tu tienda activa (real) y tu tienda de prueba (falsa) utilizando este interruptor, así que tenlo en cuenta.

Técnicamente, ni siquiera necesitas un sitio web para empezar a vender con LemonSqueezy. Genial, ¿verdad? Sólo tienes que compartir la URL de tu tienda LemonSqueezy con tus clientes, y podrán comprar tus productos directamente desde allí. ¿Qué vamos a vender? Vamos a crear nuestro primer producto y ¡a ganar dinero!

## Crear un producto

Lo primero que tenemos que hacer es ir a "Tienda"... "Productos", y hacer clic en "Nuevo producto". Como tengo un negocio de limonada, vamos a llamar al primer producto "Limonada Clásica", suave y sencillo. Para la descripción, digamos "Una limonada cítrica clásica".

Ahora podemos establecer el modelo de precios, y hay unos cuantos entre los que elegir: "Pago único", que cobra a los clientes una tarifa única, "Suscripción", que veremos en el próximo curso, "Imán de clientes potenciales", que permite el acceso gratuito, y "Paga lo que quieras", donde puedes establecer precios sugeridos y mínimos y dejar que tus clientes decidan cuánto quieren pagar por tu producto.

Optemos por la opción más sencilla, un "Pago único", y mantengamos la sencillez con el modelo "Precio estándar". Fijaré el precio en... ¿qué te parece 0,99 $? En la categoría de impuestos, tenemos que elegir una de las pocas opciones. La que mejor se adapta a nuestras necesidades es "Bienes o servicios digitales (excluidos los libros electrónicos)". Así es Nuestra limonada es digital. Como he mencionado antes, LemonSqueezy es un "Merchant of Record", por lo que limita los productos que puedes vender a través de su plataforma a sólo digitales, así que no podremos vender limonada de verdad. Qué fastidio. Pero, ¡eh! Vamos a cambiar el nombre de nuestro producto a "Limonada electrónica clásica", para que quede claro.

Bien, antes de publicar oficialmente nuestro primer producto, tenemos que asegurarnos de que no está en la lista de "Productos prohibidos". Podemos encontrarla en `docs.lemonsqueezy.com`, donde hacemos clic en "Recursos"... "Documentos de ayuda"... y más abajo, encontrarás una sección de "Productos prohibidos":

`https://docs.lemonsqueezy.com/help/getting-started/prohibited-products`.

Si sigues sin estar seguro sobre tu producto concreto después de revisar este documento, pregunta a su equipo de soporte.

Muy bien, ¡volvamos a nuestro producto en curso! ¡Un producto de éxito necesita una gran imagen! Sube una foto de nuestra deliciosa limonada... um, e-lemonada, que haga que la gente quiera machacar ese botón de "Comprar". Y ¡sorpresa! ¡Ya tenemos una! Si descargas el código del curso y lo descomprimes, dentro encontrarás un directorio `start/` con el mismo código que ves aquí. El directorio `tutorial/` tiene la imagen que necesitamos. Sólo tienes que hacer clic y arrastrarla a la sección multimedia. ¡Ohhhh sí! ¡Eso tiene un aspecto tan apetitoso que ya quiero comprarlo! Si te dedicas a la venta de archivos, puedes adjuntar un archivo al producto y tus clientes tendrán acceso a él justo después de su compra. ¡Lo mismo ocurre con los enlaces! No necesitamos ninguno para nuestra e-lemonada, así que por ahora lo dejaré vacío.

La siguiente es "Variantes". Las variantes son útiles cuando tienes un producto con diferentes opciones como sabor, tamaño, color, etc. No hay duda de que vamos a vender más de un sabor de limonada electrónica de diseño, y probablemente nuestros clientes estarán más dispuestos a comprar los otros sabores si pueden ver lo delicioso que es cada sabor, ¿verdad? Como las variantes no tienen imagen propia, tiene sentido que creemos cada sabor nuevo como un producto individual, así que lo dejaremos en blanco. Más adelante hablaremos de ello

Si vendes claves de licencia, querrás activar esto. Nosotros no lo necesitamos, así que podemos dejarlo como está. Este interruptor está activado por defecto y nos permite mostrar este producto en la página del escaparate de LemonSqueezy - útil si aún no tienes una tienda web. También puedes personalizar el "Modal de confirmación" y el "Recibo por correo electrónico", pero los valores predeterminados funcionan bien por ahora. Por cierto, LemonSqueezy gestionará todos los correos electrónicos por ti. Cada vez que un cliente compre algo en tu tienda, LemonSqueezy le enviará un correo electrónico de pedido, por lo que no tendrás que gestionar tú mismo esos complejos correos electrónicos. ¡Qué ahorro de tiempo!

Por último, vamos a "Publicar" nuestro nuevo producto. Haz clic en "Publicar producto" y... ¡ya está! ¡Acabas de crear tu primer producto LemonSqueezy! Cierra la página Detalles del producto, ¡y ahí está! ¡Tiene el estado "Publicado" y ya aparece en nuestro escaparate! ¡Qué bien!

A continuación, vamos a ver lo que ven nuestros clientes: ¡nuestro nuevo y reluciente escaparate!
