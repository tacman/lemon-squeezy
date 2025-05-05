# Iniciar la App del Proyecto de Curso

Lo entiendo perfectamente. Las palabras "integración de sistemas de pago" suenan súper intimidantes, pero créeme, al final de este curso, dirás:

> ¡Con LemonSqueezy, es facilísimo!

¡Echemos un vistazo a nuestro proyecto! Descargamos el código del curso en el capítulo anterior, pero si aún no lo has hecho, puedes descargarlo ahora desde esta página. Descomprímelo, ve al directorio `start/`, y no olvides consultar el archivo `README.md` para ver todas las instrucciones de configuración. Yo ya he completado estos pasos, así que seguiré adelante e iniciaré el servidor web con

```terminal
symfony serve -d
```

y haré clic en esta URL.

Bienvenido a "Exprime el Día", ¡nuestro puesto de limonada digital! Aquí tenemos algunos productos, podemos añadirlos a nuestro carrito, e incluso podemos especificar la cantidad. En la página del carrito, donde podemos pagar, es donde necesitaremos la integración de LemonSqueezy.

¡También ofrecemos entrega semanal de limonada a los suscriptores! Hablaremos de las suscripciones en el próximo curso. También podemos registrarnos, iniciar sesión y encontrar algunos datos básicos de la cuenta. ¡Hagámoslo!

En `AppFixtures.php`, encontrarás algunas credenciales de usuario por defecto. Copia este correo electrónico, vuelve a la página de inicio de sesión y pégalo en el campo de correo electrónico. 

[[[ code('46c671a27e') ]]]

La contraseña es "lemonpass". Pulsa "Iniciar sesión"... y echa un vistazo a la página "Cuenta". Cosas bastante básicas por el momento.

## Instalar el Cliente HTTP

Bien, ahora podemos trabajar en nuestra API. Para ayudarte a empezar, echemos un vistazo a la documentación. Ve a `lemonsqueezy.com`... haz clic en "Recursos", "Documentación para desarrolladores" y "Guías". A continuación, busca un capítulo llamado "Primeros pasos con la API". Parece que tenemos que crear una clave API (lo haremos más adelante), y luego hacer todas nuestras peticiones API utilizando el dominio específico y las cabeceras mencionadas aquí. LemonSqueezy también requiere cierta autenticación, por lo que también tendremos que pasar un token de autorización. Para enviar peticiones API desde nuestra aplicación Symfony, podemos aprovechar el componente Symfony HttpClient, que es perfecto para ejecutar peticiones HTTP.

Para ello, en tu terminal, ejecuta:

```terminal
composer require symfony/http-client
```

Parece que ya estaba instalado como dependencia indirecta, así que Composer acaba de añadirlo a `composer.json` como dependencia directa. Estupendo. Ahora tenemos que configurarlo En `config/packages/`, crea un nuevo archivo llamado`http_client.yaml`.

## Crea un cliente HTTP de alcance

En nuestro nuevo archivo, crea un cliente de ámbito que nos ayude a enviar peticiones a la API de LemonSqueezy - `framework:`, `http_client:`, `scoped_clients:` - y llámalo `lemon_squeezy.client`. A continuación, en `base_uri:`, en la sección "Realizar peticiones" de los documentos, copia esta URL - `'https://api.lemonsqueezy.com/v1/'` - y pégala aquí. A continuación, en `headers:`, cambia `Accept:` por`'application/vnd.api+json'`, y haz lo mismo con `Content-Type:`.

[[[ code('9d524fd38d') ]]]

Para la autorización, tenemos que añadir un token de portador, pero antes, vamos a configurar la clave de la API, para que podamos hacer peticiones a la API. Abre el panel de control de LemonSqueezy y ve a "Configuración", "API" y "Añadir clave API". Llamémosla "API", haz clic en "Crear clave API" y copia la clave que hemos generado. Esto es alto secreto, así que... haz como si nunca lo hubieras visto. Más adelante generaré una nueva más secreta.

Puesto que queremos mantenerla en secreto, no queremos guardarla en nuestro archivo `.env`con todo lo demás, porque eso está comprometido con el repositorio. Para mantenerlo seguro, crea un nuevo archivo `.env.local` y guárdalo allí. Este archivo no está comprometido con el repositorio, así que estará seguro aquí. Escribe `LEMON_SQUEEZY_API_KEY=` y pega nuestra clave API. 

```dotenv
# .env.local
LEMON_SQUEEZY_API_KEY={your_api_key}
```

Ahora, podemos copiar esta línea, excluyendo la clave, y en `.env`... aquí abajo... pegar. Esto realmente sólo nos permite saber que necesitamos esta variable de entorno para que nuestra aplicación funcione.

[[[ code('9b7d3f7c78') ]]]

En producción, puedes establecer esto como una variable de entorno real con tu alojamiento en la nube. O, para hacer esto aún más seguro, echa un vistazo al sistema de gestión de secretos de Symfony. Puedes encontrar más información al respecto en la documentación de `symfony.com`.

Por último, de vuelta en `http_client.yaml`, bajo `base_uri`, añade`auth_bearer: '%env(LEMON_SQUEEZY_API_KEY)%'`. ¡Listo!

[[[ code('7beaa263c3') ]]]

Ya estamos listos para enviar una petición HTTP a la API de LemonSqueezy Hagámoslo a continuación.
