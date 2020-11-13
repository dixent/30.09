project_path: /web/fundamentals/_project.yaml book_path: /web/fundamentals/_book.yaml descripción: La arquitectura del shell de la aplicación mantiene la interfaz de usuario local y carga el contenido de forma dinámica sin sacrificar la capacidad de enlace y la visibilidad de la web.

{# wf_updated_on: 2019-05-02 #} {# wf_published_on: 2016-09-27 #} {# wf_blink_components: N / A #}

# El modelo de shell de la aplicación {: .page-title}

{% incluyen "web / _shared / contributors / addyosmani.html"%}

Una arquitectura de **shell de aplicación** (o shell de aplicación) es una forma de crear una aplicación web progresiva que se carga de manera confiable e instantánea en las pantallas de sus usuarios, similar a lo que ve en las aplicaciones nativas.

El "shell" de la aplicación es el código HTML, CSS y JavaScript mínimo necesario para impulsar la interfaz de usuario y, cuando se almacena en caché sin conexión, puede garantizar **un buen rendimiento instantáneo y confiable** a los usuarios en visitas repetidas. Esto significa que el shell de la aplicación no se carga desde la red cada vez que el usuario lo visita. Solo se necesita el contenido necesario de la red.

Para [aplicaciones de una sola página](https://en.wikipedia.org/wiki/Single-page_application) con arquitecturas de JavaScript, un shell de aplicación es un enfoque de referencia. Este enfoque se basa en almacenar en caché agresivamente el shell (usando un [trabajador de servicio](/web/fundamentals/primers/service-worker/) ) para que la aplicación se ejecute. A continuación, el contenido dinámico se carga para cada página mediante JavaScript. Un shell de aplicación es útil para obtener algo de HTML inicial en la pantalla rápidamente sin una red.

<img src="images/appshell.png" alt="Arquitectura de Shell de aplicación">

Dicho de otra manera, el shell de la aplicación es similar al paquete de código que publicaría en una tienda de aplicaciones al crear una aplicación nativa. Es el esqueleto de su interfaz de usuario y los componentes básicos necesarios para que su aplicación despegue, pero probablemente no contenga los datos.

Nota: Pruebe el laboratorio de codificación de la [primera aplicación web](https://codelabs.developers.google.com/codelabs/your-first-pwapp/#0) progresiva para aprender a diseñar e implementar su primer shell de aplicación para una aplicación meteorológica. El video del [modelo Carga instantánea con el shell de la aplicación](https://www.youtube.com/watch?v=QhUzmR8eZAo) también describe este patrón.

### Cuándo usar el modelo de shell de la aplicación

Construir una PWA no significa comenzar desde cero. Si está creando una aplicación moderna de una sola página, probablemente ya esté usando algo similar a un shell de aplicación, lo llame así o no. Los detalles pueden variar un poco según las bibliotecas o marcos que esté utilizando, pero el concepto en sí es independiente del marco.

Una arquitectura de shell de aplicación tiene más sentido para aplicaciones y sitios con navegación relativamente invariable pero contenido cambiante. Varios marcos y bibliotecas de JavaScript modernos ya fomentan la división de la lógica de la aplicación de su contenido, lo que hace que esta arquitectura sea más sencilla de aplicar. Para una determinada clase de sitios web que solo tienen contenido estático, puede seguir el mismo modelo, pero el sitio es 100% shell de aplicaciones.

Para ver cómo Google creó una arquitectura de shell de aplicaciones, eche un vistazo a [Creación de la aplicación web progresiva Google I / O 2016](/web/showcase/2016/iowa2016) . Esta aplicación del mundo real comenzó con un SPA para crear una PWA que almacena en caché el contenido con un trabajador de servicio, carga dinámicamente nuevas páginas, realiza transiciones elegantes entre vistas y reutiliza el contenido después de la primera carga.

### Beneficios {: # app-shell-Benefits}

Los beneficios de una arquitectura de shell de aplicaciones con un trabajador de servicios incluyen:

- **Rendimiento confiable que es consistentemente rápido** . Las visitas repetidas son extremadamente rápidas. Los activos estáticos y la interfaz de usuario (p. Ej. HTML, JavaScript, imágenes y CSS) se almacenan en caché en la primera visita para que se carguen instantáneamente en visitas repetidas. El contenido *puede* almacenarse en caché en la primera visita, pero normalmente se carga cuando es necesario.

- **Interacciones de tipo nativo** . Al adoptar el modelo de shell de la aplicación, puede crear experiencias con navegación e interacciones instantáneas, similares a las de una aplicación nativa, con soporte fuera de línea.

- **Uso económico de datos** . Diseñe para un uso mínimo de datos y sea prudente en lo que almacena en caché porque enumerar archivos que no son esenciales (imágenes grandes que no se muestran en todas las páginas, por ejemplo) dan como resultado que los navegadores descarguen más datos de los estrictamente necesarios. Aunque los datos son relativamente baratos en los países occidentales, este no es el caso en los mercados emergentes donde la conectividad es cara y los datos son costosos.

## Requisitos {: # app-shell-requirements}

Idealmente, el shell de la aplicación debería:

- Carga rapido
- Utilice la menor cantidad de datos posible
- Usa activos estáticos de una caché local
- Separe el contenido de la navegación
- Recupere y muestre contenido específico de la página (HTML, JSON, etc.)
- Opcionalmente, caché de contenido dinámico

El shell de la aplicación mantiene su interfaz de usuario local y extrae contenido dinámicamente a través de una API, pero no sacrifica la capacidad de vinculación y descubrimiento de la web. La próxima vez que el usuario acceda a su aplicación, la última versión se mostrará automáticamente. No es necesario descargar nuevas versiones antes de usarlo.

Nota: La extensión de auditoría de [Lighthouse](https://github.com/googlechrome/lighthouse) se puede usar para verificar si su PWA usando un shell de aplicación alcanza un nivel alto de rendimiento. [To the Lighthouse](https://www.youtube.com/watch?v=LZjQ25NRV-E) es una charla que explica cómo optimizar un PWA utilizando esta herramienta.

## Construyendo el shell de su aplicación {: # building-your-app-shell}

Estructura tu aplicación para una clara distinción entre la estructura de la página y el contenido dinámico. En general, su aplicación debería cargar el shell más simple posible, pero incluir suficiente contenido de página significativo con la descarga inicial. Determine el equilibrio adecuado entre velocidad y actualización de datos para cada una de sus fuentes de datos.

&lt;figure&gt;
  &lt;img src="images/wikipedia.jpg"
    alt="Offline Wikipedia app using an application shell with content caching"&gt;
  &lt;figcaption&gt;Jake Archibald’s &lt;a href="https://wiki-offline.jakearchibald.com/wiki/Rick_and_Morty"&gt;offline Wikipedia application&lt;/a&gt; is a good example of a PWA that uses an app shell model. It loads instantly on repeat visits, but dynamically fetches content using JS. This content is then cached offline for future visits.
&lt;/figcaption&gt;
&lt;/figure&gt;

### HTML de ejemplo para un shell de aplicación {: # example-html-for-appshell}

Este ejemplo separa la infraestructura de la aplicación principal y la interfaz de usuario de los datos. Es importante mantener la carga inicial lo más simple posible para mostrar solo el diseño de la página tan pronto como se abra la aplicación web. Una parte proviene del archivo de índice de la aplicación (DOM en línea, estilos) y el resto se carga desde scripts y hojas de estilo externos.

Toda la interfaz de usuario y la infraestructura se almacenan en caché localmente mediante un trabajador de servicio para que en las cargas posteriores, solo se recuperen los datos nuevos o modificados, en lugar de tener que cargar todo.

Su archivo `index.html` en su directorio de trabajo debería parecerse al siguiente código. Este es un subconjunto del contenido real y no es un archivo de índice completo. Veamos lo que contiene.

- HTML y CSS para el "esqueleto" de su interfaz de usuario completa con navegación y marcadores de posición de contenido.
- Un archivo JavaScript externo (app.js) para manejar la navegación y la lógica de la interfaz de usuario, así como el código para mostrar las publicaciones recuperadas del servidor y almacenarlas localmente mediante un mecanismo de almacenamiento como IndexedDB.
- Un manifiesto de aplicación web y un cargador de trabajador de servicio para habilitar capacidades fuera de línea.

<div class="clearfix"></div>

```
&lt;!DOCTYPE html&gt;
&lt;html&gt;
&lt;head&gt;
  &lt;meta charset="utf-8"&gt;
  &lt;title&gt;App Shell&lt;/title&gt;
  &lt;link rel="manifest" href="/manifest.json"&gt;
  &lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;
  &lt;meta name="viewport" content="width=device-width, initial-scale=1.0"&gt;
  &lt;link rel="stylesheet" type="text/css" href="styles/inline.css"&gt;
&lt;/head&gt;

&lt;body&gt;
  &lt;header class="header"&gt;
    &lt;h1 class="header__title"&gt;App Shell&lt;/h1&gt;
  &lt;/header&gt;

  &lt;nav class="nav"&gt;
  ...
  &lt;/nav&gt;

  &lt;main class="main"&gt;
  ...
  &lt;/main&gt;

  &lt;div class="dialog-container"&gt;
  ...
  &lt;/div&gt;

  &lt;div class="loader"&gt;
    &lt;!-- Show a spinner or placeholders for content --&gt;
  &lt;/div&gt;

  &lt;script src="app.js" async&gt;&lt;/script&gt;
  &lt;script&gt;
  if (&#39;serviceWorker&#39; in navigator) {
    navigator.serviceWorker.register(&#39;/sw.js&#39;).then(function(registration) {
      // Registration was successful
      console.log(&#39;ServiceWorker registration successful with scope: &#39;, registration.scope);
    }).catch(function(err) {
      // registration failed :(
      console.log(&#39;ServiceWorker registration failed: &#39;, err);
    });
  }
  &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
```

<div class="clearfix"></div>

Nota: Consulte [Su primera aplicación web progresiva](/web/fundamentals/codelabs/your-first-pwapp/) para obtener más información sobre el uso de un shell de aplicación y la representación del lado del servidor para el contenido. Un shell de aplicación se puede implementar utilizando cualquier biblioteca o marco como se describe en nuestras <a href="https://www.youtube.com/watch?v=srdKq0DckXQ">aplicaciones web progresivas en todos los marcos</a> . Las muestras están disponibles usando Polymer ( <a href="https://shop.polymer-project.org">Shop</a> ) y React ( <a href="https://github.com/insin/react-hn">ReactHN</a> , <a href="https://github.com/GoogleChrome/sw-precache/tree/master/app-shell-demo">iFixit</a> ).

### Almacenamiento en caché del shell de la aplicación {: # app-shell-caching}

Un shell de aplicación se puede almacenar en caché mediante un trabajador de servicio escrito manualmente o un trabajador de servicio generado mediante una herramienta de almacenamiento en caché de activos estáticos como [sw-precache](https://github.com/googlechrome/sw-precache) .

Nota: Los ejemplos se proporcionan solo con fines ilustrativos e información general. Los recursos reales utilizados probablemente serán diferentes para su aplicación.

#### Almacenamiento en caché del shell de la aplicación manualmente

A continuación, se muestra un ejemplo de código de trabajador de servicio que almacena en caché los recursos estáticos del shell de la aplicación en la [API de caché](https://developer.mozilla.org/en-US/docs/Web/API/Cache) mediante el evento de `install` del trabajador de servicio:

```
var cacheName = &#39;shell-content&#39;;
var filesToCache = [
  &#39;/css/styles.css&#39;,
  &#39;/js/scripts.js&#39;,
  &#39;/images/logo.svg&#39;,

  &#39;/offline.html&#39;,

  &#39;/&#39;,
];

self.addEventListener(&#39;install&#39;, function(e) {
  console.log(&#39;[ServiceWorker] Install&#39;);
  e.waitUntil(
    caches.open(cacheName).then(function(cache) {
      console.log(&#39;[ServiceWorker] Caching app shell&#39;);
      return cache.addAll(filesToCache);
    })
  );
});
```

#### Usando sw-precache para almacenar en caché el shell de la aplicación

El trabajador de servicio generado por sw-precache almacenará en caché y servirá los recursos que configure como parte de su proceso de compilación. Puede hacer que guarde en caché todos los archivos HTML, JavaScript y CSS que componen el shell de su aplicación. Todo funcionará sin conexión y se cargará rápidamente en visitas posteriores sin ningún esfuerzo adicional.

Aquí hay un ejemplo básico del uso de sw-precache como parte de un proceso de compilación de [gulp](http://gulpjs.com) :

```
gulp.task(&#39;generate-service-worker&#39;, function(callback) {
  var path = require(&#39;path&#39;);
  var swPrecache = require(&#39;sw-precache&#39;);
  var rootDir = &#39;app&#39;;

  swPrecache.write(path.join(rootDir, &#39;service-worker.js&#39;), {
    staticFileGlobs: [rootDir + &#39;/**/*.{js,html,css,png,jpg,gif}&#39;],
    stripPrefix: rootDir
  }, callback);
});
```

To learn more about static asset caching, see the [Adding a Service Worker with sw-precache](https://codelabs.developers.google.com/codelabs/sw-precache/index.html?index=..%2F..%2Findex#0) codelab.

Nota: sw-precache es útil para almacenar en caché sin conexión sus recursos estáticos. Para recursos dinámicos / en tiempo de ejecución, recomendamos utilizar nuestra [caja de herramientas sw de](https://github.com/googlechrome/sw-toolbox) biblioteca gratuita.

## Conclusión {: #conclusion}

Un shell de aplicación que usa Service worker es un patrón poderoso para el almacenamiento en caché sin conexión, pero también ofrece ganancias de rendimiento significativas en forma de carga instantánea para visitas repetidas a su PWA. Puede almacenar en caché el shell de su aplicación para que funcione sin conexión y completar su contenido con JavaScript.

En visitas repetidas, esto le permite obtener píxeles significativos en la pantalla sin la red, incluso si su contenido finalmente proviene de allí.

## Retroalimentación retroalimentación }

{% incluyen "web / _shared / helpful.html"%}
