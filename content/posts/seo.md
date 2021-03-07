---
title: "Seo"
date: 2021-02-20T22:33:55-03:00
draft: false
tags: ['seo', 'hugo', 'google search']
---

# Para qué entender SEO?

A la hora de administrar sitios webs, hay que pensar en como optimizar el sitio para que sea indexado por buscadores.
El tema es complejo, pero voy a entender y explicar las bases de que es y como optimizar el sitio para seo.
Facilitar que el sitio sea fácil de descubrir ayuda a traer tráfico al sitio.
A grandes rasgos el SEO se basa en entender el contenido que el público al que apuntamos busca.
Así como también lograr que los crawlers puedan interpretar nuestro contenido para ofrecerlo al público esperado.

Para poder mejorar esto, se debe considerar:

1.  El sitio debe poder ser revisado por los web-crawlers de los buscadores
2.  El contenido debe responder las preguntas del buscador
3.  Se deben utilizar palabras claves para optimizar para atraer buscadores
4.  Buena experiencia de usuario
5.  Contenido de calidad, que pueda ser citado y amplificado
6.  Titulo, url y descripción para subir el indice CTR (click-through ratio)
7.  Utilización de funcionalidades para subir el indice SERPs (search engine result pages)

# Vale la pena?

Todo el trabajo requerido para realizar estás implementaciones y mejoras es para atraer mayor cantidad de tráfico a tu sitio.
Por lo que, dependiendo de cuanto tráfico quieras atraer a tu sitio, es el esfuerzo que hay que dedicarle al SEO.
La idea general es que mientras mejor optimicemos el SEO, más cerca de ser el primer resultado orgánico (no un anuncio pago).
Dependiendo del modelo de negocio utilizado, cambia la prioridad de implementar SEO.

# Resultados orgánicos

A diferencia de los anuncios, que son los primeros en aparecer, los resultados orgánicos son aquellos que dependen de la implementación SEO utilizada.
Aquí es donde veremos la mayor ganancia del seo; que a diferencia de los anuncios pagos, nos otorgan un flujo de usuarios más sostenido a lo largo del tiempo.
Los anuncios pagos de google, si bien efectivos, solo duran a lo largo de la campaña determinada y el ctr es bajo.
En cambio un SEO bien implementado nos va a permitir posicionar el sitio de manera efectiva, y dar un mejor retorno a lo largo del tiempo.
Ya que la implementación y mantenimiento adecuado nos permiten mantener el sitio bien posicionado, generando mayor tráfico a nuestro contenido.
Cuando en la búsqueda de optimizar el SEO, uno implementa técnicas que buscan engañar a los motores de búsqueda, puede recibir penalizaciones. Causando una caída en la indexación del sitio.

# Primeros pasos

En esta sección voy a hablar de sitios estáticos generados con Hugo, debido a que es el flujo de trabajo que manejo para este blog.

## Cloudflare

Cloudflare es un servicio de DNS (domain name service) que nos va a permitir gestionar como los usuario acceden a nuestro sitio.
A su vez, ofrece un servicio de CDN (content delivery network), lo que aumenta la velocidad de respuesta de nuestro sitio.
Finalmente nos otorga la posibilidad de poner certificados ssl (propios u otorgados por cloudflare), para poder exponer el sitio a través de https.
La importancia de tener un certificado ssl en el sitio, brindar el contenido a través de https y a través de un CDN; no solo mejora nuestro ranking seo, si no la percepción del usuario final que accede a un sitio seguro y más rápido.
No todos los usuarios le van a prestar atención a esto, pero a menos que el usuario ya conozca el sitio, si el navegador arroja un mensaje de error por la falta de cifrado o un problema de certificado, lo más probable es que el usuario se vaya del sitio.
Se puede sacar una cuenta gratuita para realizar todas las gestiones, y obtener algunas herramientas adicionales sin costo.
También nos permite, en el caso de Hugo, publicar el contenido creado directamente en la web de ellos, con un tier gratuito.
Esto permite que en caso de webs que buscan monetizar, no tener que realizar una inversión antes de poder generar ingresos para sostenerla.
Para más información sobre los servicios y costos de cloudflare, puede hacer click [aquí](https://www.cloudflare.com/).

## Creación de robots.txt

Este archivo le indica a los motores de búsqueda que partes de tu web pueden indexar y cuales no.
Si el acceso a esa web está protegido por un password, tampoco es indexado.

*Nota:* esto es relevante si estás usando gestores de contenido con paneles de administración como wordpress.
El mover el panel de administración a una url no estándar y poniéndole password a la página; evita que el panel de administración sea indexado y aparezca en búsquedas.

Para que hugo cree este archivo, debemos agregar lo siguiente a nuestro archivo de configuración:

```
enableRobotsTXT = true
```

Algunas limitaciones de esta técnica son:

- No todos los motores de búsqueda la soportan
- Dependiendo del crawler utilizado pueda cambiar la manera de interpretar la sintaxis
- Si otro sitio pone un link al tuyo, por más que figure en robots.txt será indexado

Para mayores detalles de como funciona esto, se debe acceder a la documentación del motor de búsqueda objetivo.
Para leer más información de como google gestiona esto, puede presionar [aquí](https://developers.google.com/search/docs/advanced/robots/intro).

## Creación del sitemap.xml

La creación del sitemap, permite a los crawlers tener mejor entendimiento de la estructura del sitio.
Los mayores beneficios de está técnica, se pueden observar cuando:

- Se crean páginas dinámicas
- La estructura y los links internos del sitio presentan errores
- El sitios es nuevo o tiene pocas referencias desde links externos
- Hay mucho contenido archivado

Es importante que validemos el contenido del sitemap.
Podemos encontrar información relevante del seo de nuestro sitio a través de:

- [google](https://search.google.com/search-console/about)
- [bing](https://www.bing.com/toolbox/webmaster/)
- [duckduckgo](https://webmaster.yandex.com/welcome/)

En el caso de encontrar errores en el sitemap, se debe proceder a la corrección manual de los mismos.
En todos los casos, los sitios nombrados, requieren registrar una cuenta y verificación del dominio.

*Nota:* la ruta para la versión manual del sitemap.xml es layouts/sitemap.xml

## Creación de estructura de datos (seo_schema)

Esta herramienta nos permite darle una estructura al contenido de nuestro sitio.
Dicha estructura permite que google pueda acceder al contenido del mismo y exponerlo a través de las funciones especiales del buscador.
Para esto debemos incorporaren la sección layouts/partials/header.html

```
{{ partial "seo_schema" . }}
```

Luego debemos incluir el archivo que llamamos en la misma carpeta, layouts/partial/seo_schema.html .
El ejemplo sería como el siguiente:

```
<script type="application/ld+json">
{
    "@context" : "http://schema.org",
    "@type" : "BlogPosting",
    "mainEntityOfPage": {
        "@type": "WebPage",
        "@id": "{{ .Site.BaseURL }}"
    },
    "articleSection" : "{{ .Section }}",
    "name" : "{{ .Title }}",
    "headline" : "{{ .Title }}",
    "description" : "{{ if .Description }}{{ .Description }}{{ else }}{{if .IsPage}}{{ .Summary }}{{ end }}{{ end }}",
    "inLanguage" : "es-AR",
    "author" : "{{ range .Site.Author }}{{ . }}{{ end }}",
    "creator" : "{{ range .Site.Author }}{{ . }}{{ end }}",
    "publisher": "{{ range .Site.Author }}{{ . }}{{ end }}",
    "accountablePerson" : "{{ range .Site.Author }}{{ . }}{{ end }}",
    "copyrightHolder" : "{{ range .Site.Author }}{{ . }}{{ end }}",
    "copyrightYear" : "{{ .Date.Format "2021" }}",
    "datePublished": "{{ .Date }}",
    "dateModified" : "{{ .Date }}",
    "url" : "{{ .Permalink }}",
    "wordCount" : "{{ .WordCount }}",
    "keywords" : [ {{ if isset .Params "tags" }}{{ range .Params.tags }}"{{ . }}",{{ end }}{{ end }}"Blog" ]
}
</script>
```

# Conclusión

Si bien esto es una pequeña introducción al complejo mundo del SEO, la idea es expandir el articulo con nuevas secciones.
Esto permitirá documentar el proceso de mejora del SEO del sitio, e incorporar las experiencias a medida de que incorporó nuevas optimizaciones.
Finalmente, quisiera dejar como referencia, parte del material de utilidad que fui leyendo para la creación del articulo en español.

- [SEO para Hugo](https://www.mybluelinux.com/hugo-website-seo/)
- Recursos de google
    - [Introducción a SEO](https://developers.google.com/search/docs/guides/get-started)
    - [Introducción a datos estructurados](https://developers.google.com/search/docs/guides/intro-structured-data#search-appearance)
    - [Sitemaps](https://developers.google.com/search/docs/advanced/sitemaps/overview?hl=es)
- [Guía de principiantes para SEO](https://moz.com/beginners-guide-to-seo)
- [Documentación de Hugo](https://gohugo.io/documentation/)
