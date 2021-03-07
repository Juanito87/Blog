---
title: "Seo"
date: 2021-02-20T22:33:55-03:00
draft: true
tags: ['seo', 'hugo']
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

# Primeros pasos

En esta sección voy a hablar de sitios estáticos generados con Hugo, debido a que es lo que estoy usando.

## Creación de robots.txt

Este archivo le indica a los motores de búsqueda que partes de tu web pueden indexar y cuales no.
Si el acceso a esa web está protegido por un password, tampoco es indexado.

*Nota:* esto es relevante si estás usando gestores de contenido con paneles de administración como wordpress.
El mover el panel de administración a una url no estándar y poniéndole password a la página; evita que el panel de administración sea indexado y aparezca en búsquedas.

Para que hugo creee este archivo, debemos agregar lo siguiente a nuestro archivo de configuración:

```
enableRobotsTXT = true
```

Algunas limitaciones de esta técnica son:

- No todos los motores de busqueda la soportan
- Dependiendo del crawler utilizado pueda cambiar la manera de interpretar la sintaxis
- Si otro sitio pone un link al tuyo, por más que figure en robots.txt será indexado

Para mayores detalles de como funciona esto, se debe acceder a la documentación del motor de busqueda objetivo.
Para leer más información de como google gestiona esto, puede presionar [aquí](https://developers.google.com/search/docs/advanced/robots/intro).
