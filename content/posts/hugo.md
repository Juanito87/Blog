---
title: "Sitios estáticos con hugo"
date: 2021-03-15T22:19:53-03:00
draft: false
---
# Cómo hostear un sitio web de manera gratuita y sencilla?

Este proceso es bastante sencillo, y no se requieren demasiados conocimientos técnicos.
Utilizaremos las siguientes herramientas:

* Sitios estáticos con hugo para el desarrollo del sitio
* Cloudflare para gestionar el dns y hostear el contenido
* Github para mantener un respaldo del sitio y automatizar las actualizaciones
* SEO para mejorar como es indexado el sitio

# Hugo

Con este software podemos crear sitios estáticos, que luego podemos publicar a cloudflare o github pages.
Para poder realizar las gestiones en principio hay que definir:

- El tema a utilizar
- Las configuraciones a habilitar
- Las personalizaciones adicionales a las del tema

Para poder trabajar en dichos elementos deberemos:

1. Configurar Hugo a través de config.toml en la raíz del repo
2. Agregar los cambios del layout que queramos persistir en las carpetas de la web y no del tema

Una vez hechas las configuraciones, podemos utilizar la línea de comandos para generar una versión del sitio para visualizar.
Para esto realizaremos:

`hugo server -D`

Esto generará un borrador en memoria del sitio, incluyendo los borradores, y se podrá visualizar en localhost:1313
Otro parámetro útil es --renderToDisk, que nos generará los archivos a la ruta que indiquemos.
Esto nos permite, por ejemplo, sacar una copia del archivo sitemap.xml, y aplicarle las personalizaciones que querramos, en vez de usar el autogenerado.

Para incorporar nuevo contenido se debe ejecutar:

`hugo new post/post.md`

De está manera se crea la plantilla para el nuevo posteo de acuerdo a la configuración.

## Tema

El tema puede ser creado desde 0 por el usuario, o se pueden implementar  temas hechos por usuarios.
A estos podemos acceder a una galería para definir si los queremos implementar [aquí](https://themes.gohugo.io/)
Una vez definido el tema a utilizar hay que configurarlo.
En el caso de los temas predefinidos hay que seguir las configuraciones indicadas por el tema.
Adicionalmente de las definiciones que tomemos.
El sitios actualmente usa el tema [terminal](https://themes.gohugo.io/hugo-theme-terminal/).
Es importante la correcta indentación del archivo de configuración para que el sitio pueda ser rendereado de manera correcta.

El código necesario para que funcione es el siguiente:

```
theme = "terminal"
paginate = 5

[params]
# dir name of your main content (default is `content/posts`).
# the list of set content will show up on your index page (baseurl).
    contentTypeName = "posts"

# ["orange", "blue", "red", "green", "pink"]
    themeColor = "orange"

# if you set this to 0, only submenu trigger will be visible
    showMenuItems = 2

# show selector to switch language
    showLanguageSelector = false

# set theme to full screen width
    fullWidthTheme = false

# center theme with default width
    centerTheme = false

# set a custom favicon (default is a `themeColor` square)
# favicon = "favicon.ico"

# set post to show the last updated
# If you use git, you can set `enableGitInfo` to `true` and then post will automatically get the last updated
    showLastUpdated = false
# Provide a string as a prefix for the last update date. By default, it looks like this: 2020-xx-xx [Updated: 2020-xx-xx] :: Author
# updatedDatePrefix = "Updated"

# set all headings to their default size (depending on browser settings)
# it's set to `true` by default
# oneHeadingSize = false

    [params.twitter]
# set Twitter handles for Twitter cards
# see https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started#card-and-content-attribution
# do not include @
    creator = ""
    site = ""

[languages]
    [languages.en]
        languageName = "English"
        title = "Terminal"
        subtitle = "A simple, retro theme for Hugo"
        owner = ""
        keywords = ""
        copyright = ""
        menuMore = "Show more"
        readMore = "Read more"
        readOtherPosts = "Read other posts"
        missingContentMessage = "Page not found..."
        missingBackButtonLabel = "Back to home page"

    [languages.en.params.logo]
        logoText = "Terminal"
        logoHomeLink = "/"

    [languages.en.menu]
    [[languages.en.menu.main]]
        identifier = "about"
        name = "About"
        url = "/about"
    [[languages.en.menu.main]]
        identifier = "showcase"
        name = "Showcase"
        url = "/showcase"
```

*Nota:* La parte de lenguajes puede ser modificada, para más detalles, ver en el código.

Para lograr el correcto funcionamiento del sitio es importante validar que el archivo de configuración utilice la indentación correcta.

## SEO
Para configurar los componentes de SEO del sitio tenemos varias consideraciones que realizar.
Definir las taxonomías de manera explicita, permite definir cuales deben usarse y cuales no.

```
# Taxonomies // definición de la sección
[taxonomies]
# Definición de las taxonomias a utilizar
    tag = "tags"
```

Si no por defecto se utilizan tags y categories.
También podemos utilizar taxonomías personalizadas que hay que definir en la configuración:

```
series = "series"
```

A la hora de definir taxonomías personalizadas, se debe considerar la forma
llave singular = valor plural
Para más información de taxonomías, la documentación de Hugo la puede encontrar [aquí](https://gohugo.io/content-management/taxonomies/)

En mi caso, utilizo tags para mis posteos.

Para que el archivo sitemap.xml sea formado de manera automática de manera correcta, en el archivo config.toml, se debe modificar las siguientes líneas:

```
baseURL = "https://yoursite.com/"
canonifyURLs = true
disableSitemap = false
[sitemap]
    changefreq = "monthly"
    filename = "sitemap.xml"
    priority = 0.5
```

El protocolo debe ser incluido en la línea y se recomienda utilizar https, ya que esto otorga mejor ranking en SEO.
Canonifyurls se utiliza para transformar las urls relativas en absolutas.

Finalemente, habilitaremos la configuraciónpara habilitar los agentes de parseo de los buscadores con la siguiente línea:

`enableRobotsTXT = true`

Para ver más parámetros para hugo, puede consultar [aquí](https://gohugo.io/getting-started/configuration/#all-configuration-settings)

# Qué es Cloudflare?

Es un proveedor de servicios en la nube, con un foco en mejorar internet facilitando el acceso de los usuarios finales a:

- Redes de entrega de contenido (CDN), que implica sitios más rápidos
- Certificados ssl, que implica sitios más seguros
- Gestión de dominios (DNS) simplificada a través de una GUI
- Etc

La totalidad de los servicios de cloudflare es amplía; también tienen tiers pagos y servicios orientados a empresas.
Pero en líneas generales, si uno esta buscando empezar un negocio, o aprender, cloudflare ofrece la oportunidad de hacerlo.
Para poder gestionar el blog, utilizaremos los servicios mencionados arriba.
Para más información de cloudflare, [aquí](https://www.cloudflare.com/)

## DNS

Lo primero que necesitamos es comprar un dominio para poder publicar el sitio.
Donde comprar el dominio va a depender de varias cosas, pero en mi caso, lo adquirí a través de [nic.ar](https://nic.ar/) (el ente que regular los dominios en argentina).
El costo es barato, y teniendo el dominio podemos hostear el blog o cualquier otro servicio con el que querramos experimentar.
Como hosting utilizo los workers de cloudflare, por lo que solo necesito crear un cname para el blog dentro del dominio y cloudflare se encarga de manera automatizada de resolver el nombre de dominio dentro de su red.


## Certificados ssl

Al estar hosteando el sitio como contenido estático dentro del cdn, utilizamos los certificados que cloudflare otorga dentro de su red. Esta gestión también se hace de manera automatizada.
Si el sitio estuviera hosteado en un servicio externo, tendríamos que generar e instalar el certificado en nuestro servidor para encriptar la comunicación de manera end-to-end.
O podríámos optar por realizar un proceso automatizado con let's encrypt.
A la hora de gestionar certificados, cloudflare por defecto emite certificados para el dominio y los subdominios.
Pero también permite la utilización de certificados entregados por otras entidades; el proceso consiste en subir el archivo dentro de tu cuenta, para que cloudflare lo gestione de manera adecuada.

## Hosting

Para hostear el sitio en cloudflare, utilizaremos una herramienta llamada workers.
Esta nos permite hostear contenido estático y aplicaciones serverless de manera sencilla utilizando wrangler.
Podemos realizar 100.000 request gratis por día, luego nos cobrán por el servicio.
Si bien en el caso de mi blog no es relevante, una de las grandes ventajas de esté tipo de deployment, es que es global.
La otra ventaja de este enfoque es que no hay que saber administrar servidores, porque no hay que saber hacerlo

# Qué es github?

Github es un repositorio de git remoto.
Esto nos permite tener un respaldo externo del código con el que trabajemos fuera de casa.
Permitiendonos recuperar el trabajo en caso de:

* Perdida
* Robo
* Ataque de malware
* Etc

El repositorio de git a su vez, nos permite llevar el control de los distintos cambios que realizamos en nuestro código.
Adicionalmente contamos con servicios para:

* Automatización (github actions)
* Documentación (wiki)
* Gestión de incidencias (issues)
* Planificación (projects)

Todo estó, orientado al trabajo colaborativo. Por lo que podemos definir si los repositorios son públicos o privados.
Quien puede tomar acciones en nuestro repositorio. Distintos niveles de responsabilidad para los integrantes del equipo.
Y varias cosas más, por ejemplo gestión de archivos de configuración, sobre lo que podes leer más [aquí](https://blog.juanito87.com.ar/posts/manejo-de-dotfiles/)
Para la gestión del blog nos interesan fundamentalmente 2 cosas:

## Repositorio

Aquí es donde guardaremos y versionaremos toda la información que publicamos con el sitio:

- Imágenes
- Contenido
- Configuración
- Etc

Hay que considerar que si el repositorio es público, debemos eliminar la información sensible de los archivos de configuración.
Si el repositorio es privado no es estrictamente necesario pero es una buena práctica.
Si vas a utilizar la automatización del deployment a cloudflare, es recomendable migrar la información sensible a github secrets.
Esto sera utilizado en el paso siguiente, cuando interactuemos con github actions.

## Automatización

Para automatizar la publicación del blog cada vez que hago un cambio en el repositorio, utilizaré github action y secrets.
En el caso de github secrets, nos permite almacenar información sensible de manera segura, y llamarla a la hora de ejecutar una acción como si fuera una variables.
Una vez definidos los secretos, nos queda definir el flujo de acciones que se tomaran para realizar la publicación del sitio.
Para eso, utilizaremos el siguiente script:

```
# Nombre del flujo
name: Deploy to cloudflare

# Cuando vamos a realizar las acciones
on:
# El evento que gatilla el flujo (push de nuevo contenido al repositorio)
    push:
# Los detalles del evento (solo cuando el push es en la rama main)
        branches: [ main ]

# Las tareas que se vana  realizar en el flujo
jobs:
# La o las tareas a realizar
    build:
# El tipo de servicio en el que se van a correr las tareas.
        runs-on: ubuntu-latest

# Definimos los pasos a realizar para alcanzar nuestro cometido (publiccar el blog)
    steps:
# Se realiza un checkout del repositorio en $GITHUB_WORKSPACE, para que el job pueda acceder al contenido
        - uses: actions/checkout@v2
          with:
# Especificaciones de como se debe ejecutar la acción
            submodules: true
            fetch-depth: 1

# Instalar la última versión de hugo
        - name: Setup Hugo
          uses: peaceiris/actions-hugo@v2
          with:
            hugo-version: 'latest'

# Construir el sitio estático para poder publicar el contenido
        - name: Build site
          run: hugo

# Publicarlo a los workers de cloudflare
        - name: Publish to cloudflare
          uses: cloudflare/wrangler-action@1.3.0
          env:
# Definición de variables de entorno
# Las definidas ${{ secrets.algo }} son las almacenadas en secrets
            USER: root
            CF_ACCOUNT_ID: ${{ secrets.CF_ACCOUNT_ID }}
            CF_ZONE_ID: ${{ secrets.CF_ZONE_ID }}
          with:
            apiToken: ${{ secrets.CF_API_TOKEN }}
```

Una vez que la ejecución finaliza, podremos accerder a la última versión del sitio en la url que hayamos definido.
En mi caso https://blog.juanito87.com.ar

# SEO

Esto nos permitirá mejorar la manera en la que el sitio es indexado por los buscadores.
Hay herramientas especificas para cada buscador, pero las configuraciones suelen no variar entre si.
Para tener un buen ranking de seo hay dos objetivos separados:

* El usuario final
* Los robots que hacen el index

## Contenido

Lo importante respecto al contenido es que sea relevante y de calidad para la audiencia a la que uno apunta.
La incorporación de imágenes únicas, contenido desarrollado en profundidad y/o que responde preguntas concretas ayuda a mejorar el ranking.
Lograr que nos linkeen desde otros sitios también va a ayudar a calificar nuestro sitio, por lo que buscar sinergia con otros creadores de contenido ayuda. Incluso si el medio es es distinto (youtube, un podcast, etc.)
Finalmente, es importante que el sitio tenga un buen rendimiento, y que los usuarios que llegan al sitio a través del navegador, se queden la mayor cantidad de tiempo posible.

Para profundizar un poco más al respecto, pueden leer [aquí](https://blog.juanito87.com.ar/posts/seo/)
