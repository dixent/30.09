---
"$title": Precargar componentes de bloqueo de renderizado
"$order": '40'
tags:
- lcp
- defensor
---

Permita que los usuarios vean e interactúen con el contenido lo antes posible cargando previamente los componentes que pueden bloquear el primer procesamiento. Los componentes de bloqueo de renderización a tener en cuenta incluyen [`amp-experiment`](https://amp.dev/documentation/components/amp-experiment/?format=websites) y [`amp-dynamic-css-classes`](https://amp.dev/documentation/components/amp-dynamic-css-classes/) . Precarguelos incluyendo el `rel="preload"` en su script de importación:

```
<link rel="preload" as="script" href="https://cdn.ampproject.org/v0/amp-experiment-0.1.js">
```

¡Utilice un [AMP Optimizer](https://amp.dev/documentation/guides-and-tutorials/optimize-and-measure/amp-optimizer-guide/) para hacerlo automáticamente!
