# p8-t3-jekyll-netlify-alu0100836950

* Alexis Rodríguez Casañas
* Alberto Martín Núñez


## Índice de contenido
1. [Introducción](#id1)
    1. [Step by step tutorial](#id1.1)
        1. [Setup](#id1.1.1)
        2. [Liquid](#id1.1.2) 
        3. [Front Matter](#id1.1.3) 
        4. [Layouts](#id1.1.4) 
        5. [Includes](#id1.1.5) 
        6. [Data Files](#id1.1.6)
        7. [Assets](#id1.1.7) 
        8. [Blogging](#id1.1.8) 
        9. [Collections](#id1.1.9)    
2. [Despliegue en GitHub Pages](#id2)
3. [Navigation](#id3)
4. [404](#id4)
5. [Themes](#id5)
6. [Test the Deployment with html-profer and Travis](#id6)
7. [CI with Travis](#id7)
8. [Despliegues de Jekyll en Netlify](#id8)

### Introduccion <a name="id1"></a>

**Jekyll** es un programa que necesita tener instalado **ruby** previamente. Este programa se utiliza para crear sitios web estáticos. Vamos a seguir el siguiente tutorial para realizar los primeros pasos que debemos hacer para utilizar **jekyll**.

#### Step by step tutorial <a name="id1.1"></a>
##### Setup <a name="id1.1.1"></a>
##### Installation

Una vez comprobado que tenemos ruby instalado y actualizado podemos instalar jekyll ejecutando el siguiente comando en nuestra consola:

`gem install jekyll bundler`

Posteriormente para tener una lista de dependecias de nuestro proyecto vamos a ejecutar  `bundle init`. Con esto hemos creado un fichero lllamado *Gemfile* en el cual añadiremos lo siguiente:

`gem "jekyll"`

Esto significa que jekyll se ha añadido como una dependencia

Ahora podemos ejecutar `bundle` para installar jekyll en nuestro proyecto.

##### Create a site

Crearemos un nuevo directorio para nuestro sitio. Este será nuestro directorio "raíz", en nuestro caso lo llamaremos *practica_jekyll* y entro de él crearemos su primer archivo *index.html* con el siguiente contenido:

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Home</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

##### Build

Jekyll es un generador de sitio estático, por lo que necesitamos que Jekyll construya el sitio antes de que podamos verlo. Hay dos comandos que puede ejecutar para construirlo:

- `jekyll build` -> Construye el sitio y genera un sitio estático en un directorio llamado *_site*.
- `jekyll serve`-> Hace lo mismo, excepto que se reconstruye cada vez que realiza un cambio y ejecuta un servidor web local en http://localhost:4000.

En este caso ejecutaremos el segundo comando y navegaremo hasta *localhost:4000* para comprobar que nuestro "hola mundo" se ha creado correctamente.

<img  src = "/img/hello_world.png" alt ="Hello world" />

##### Liquid <a name="id1.1.2"></a>

**Liquid** es un lenguaje de plantillas que tiene tres partes principales: *objects*, *tags* y *filters*.

- Objects

Los objetos le dicen a *Liquid* dónde dar salida al contenido y estos sstán denotados por llaves dobles: {{ }}. 

Por ejemplo:

`{ page.title }}`

Emite una variable llamada *page.title* en la página.

- Tags

Las etiquetas crean la lógica y el flujo de control para las plantillas. Se denotan con llaves y signos de porcentaje: {%  %}. Por ejemplo:

```
{% if page.show_sidebar %}
  <div class="sidebar">
    sidebar content
  </div>
{% endif %}
```

Esto emite la barra lateral si *page.show_sidebar* es verdadero.

- Filters

Los filtros cambian la salida de un objeto liquid. Se usan dentro de una salida y están separados por *|*. Por ejemplo:

`{{ "hi" | capitalize }}``

Salida -> *Hi*

**USANDO LIQUID**

Ahora modificaremos nuestro "Hello World de la siguiente forma para que el texto salgo en minúsculas:

   ```html 
   <h1>{{ "Hello World!" | downcase }}</h1>

   ```

Para que Jekyll procese nuestros cambios, necesitamos agregar a la parte superior de la página una cierta información, este espacio se llama **front matter**.

##### Front Matter <a name="id1.1.3"></a>

El **front matter** es un fragmento YAML que se encuentra entre dos líneas de triple trazo en la parte superior de un archivo. Se utiliza para establecer variables para la página, por ejemplo:

```

---
my_number: 5
---
```

Las variables del **front matter** están disponibles en Liquid bajo la variable **page**. Por ejemplo, para generar la variable anterior, usaría:

`{{ page.my_number }}`

**USANDO EL FRONT MATTER**

Vamos a cambiar el <title> de nuestra página utilizando el **front matter**

```html
---
title: Home
---
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    <h1>{{ "Hello World!" | downcase }}</h1>
  </body>
</html>
```


##### Layouts <a name="id1.1.4"></a>

Los sitios web suelen tener más de una página y para ello **Jekyll** admite *Markdown* y *HTML* para páginas. 

Crearemos *about.md* en la raíz.

**CREANDO UN LAYOUT**

Los diseños son plantillas que envuelven su contenido y se almacenan en un directorio llamado *_layouts*.

Crearemos nuestro primer diseño en *_layouts/default.html* con el siguiente contenido:

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    {{ content }}
  </body>
</html>
```

Como vemos es casi idéntico a *index.html* excepto que no hay un tema principal y el contenido de la página se reemplaza por una variable **content**. 

**Content** es una variable especial que tiene el valor del contenido representado de la página a la que se llama.

El diseño envuelve el contenido de la página, por lo que todo lo que debe de haber en *index.html* es:

```
---
layout: default
title: Home
---
<h1>{{ "Hello World!" | downcase }}</h1>
```

**ABOUT PAGE**

Crearemos en el direcotrio raiz de nuestro proyecto el fichero *about.md* con el mismo diseño que *index.html*:

```
---
layout: default
title: About
---
# About page

This page tells you a little bit about me.
```

Hemos creado otra pagina asi que ahora podremos abrir nuestro navegador e introducir *http: //localhost: 4000/about.html*. 

<img  src = "/img/about.png" alt ="Page about" />

##### Includes <a name="id1.1.5"></a>

Estamos formando el sitio con varias paginas, sin embargo, no hay forma de navegar entre páginas. 

**INCLUDE TAG**

La etiqueta *include* nos permite incluir contenido de otro archivo almacenado en una carpeta *_includes*. 

**INCLUDE USAGE**

Cree un archivo para la navegación en *_includes/navigation.html* con el siguiente contenido:

```html
<nav>
  <a href="/">Home</a>
  <a href="/about.html">About</a>
</nav>
```

Usemos la etiquetas para añadir la navegación a *_layouts/default.html*:

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
  </head>
  <body>
    {% include navigation.html %}
    {{ content }}
  </body>
</html>
```
Ahora deberíamos de ir  *http://localhost:4000* y poder navegar entre las páginas.

<img  src = "/img/navigation.png" alt ="Navigation page" />

**CURRENT PAGE HIGHLIGHTING**

Vamos resaltemos la página actual en la navegación.

**includes/navigation.html** necesita saber la URL de la página en la que está insertada para poder agregar estilo. Jekyll tiene variables útiles disponibles y una de ellas es **page.url**.

Utilizando **page.url** puede verificar si cada enlace es la página actual y colorearlo de rojo si es verdadero:

```html
<nav>
  <a href="/" {% if page.url == "/" %}style="color: red;"{% endif %}>
    Home
  </a>
  <a href="/about.html" {% if page.url == "/about.html" %}style="color: red;"{% endif %}>
    About
  </a>
</nav>
```

<img  src = "/img/page_highlighting.png" alt ="Page highlighting" />

Ahora podremos navegar entre las páginas que hemos creado y ver como se marca en rojo la pagina actual que estamos viendo.


##### Data Files <a name="id1.1.6"></a>

**Jekyll** admite la carga de datos de archivos *YAML*, *JSON* y *CSV* ubicados en un directorio **_data** .

Los archivos de datos son una excelente manera de separar el contenido del código fuente para hacer que el sitio sea más fácil de mantener.

En este paso almacenaremos el contenido de la navegación en un archivo de datos y luego iterará sobre él.

**DATA FILE USAGE**

**YAML** es un formato común en Ruby y en esta ocasión lo usaremos para almacenar una variedad de elementos de navegación, cada uno con un *nombre* y un *enlace*.

Para hacerlo crearemos un archivo de datos para la navegación en *_data/navigation.yml* con lo siguiente:

- name: Home
  link: /
- name: About
  link: /about.html

Jekyll pone a su disposición este archivo de datos en *site.data.navigation*. En lugar de mostrar cada enlace, ahora podemos iterar sobre el archivo de datos *_includes/navigation.html*:

```html
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}style="color: red;"{% endif %}>
      {{ item.name }}
    </a>
  {% endfor %}
</nav>
```

##### Assets <a name="id1.1.7"></a>

Usar CSS, JS, imágenes y otros activos es sencillo con Jekyll, normalmente se usa esta estructura para mantener los activos organizados:

<img  src = "/img/struct_assets.png" alt ="Estructura de assets" />

**SASS**

Los estilos en línea utilizados en *_includes/navigation.html* no son una práctica recomendada, en vez de eso diseñaremos la página actual con una clase.

```html
<nav>
  {% for item in site.data.navigation %}
    <a href="{{ item.link }}" {% if page.url == item.link %}class="current"{% endif %}>{{ item.name }}</a>
  {% endfor %}
</nav>
```

Para diseñar la página vamos a usar **Sass**, una extensión(preprocesador) para CSS.

Primero vamo a crear un archivo Sass en */assets/css/styles.scss* con el siguiente contenido:

```
---
---
@import "main";
```

El **front matter** vacío en la parte superior le dice a Jekyll que necesita procesar el archivo. El `@import "main"` le dice a **Sass** que busque un archivo llamado *main.scss* en el directorio sass, (*_sass/* por defecto).

Crearemos un archivo Sass -> */_sass/main.scss* con el siguiente contenido:

```css
.current {
  color: green;
}
```
Una vez tengamos nuestra hoja de estilo tendremos que indicar que queremos usarla en nuestro sitio web:

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>{{ page.title }}</title>
    <link rel="stylesheet" href="/assets/css/styles.css">
  </head>
  <body>
    {% include navigation.html %}
    {{ content }}
  </body>
</html>
```

Como podemos ver en la siguiente imagen ahora el texto aparece en verde ya que estamos utilizando la hoja de estilo nueva que hemos creado.

<img  src = "/img/style.png" alt ="Site web green words" />

##### Blogging <a name="id1.1.8"></a>

**POSTS**

Las publicaciones de blog se almacenan en una carpeta llamada **_posts**. El nombre de archivo para las publicaciones tiene un formato especial: 

1. Fecha de publicación 
2. Un título
3. Una extensión

Crearemos nuestra primera publicación en *_posts/2018-08-20-bananas.md* con el siguiente contenido:

```
---
layout: post
author: jill
---
A banana is an edible fruit – botanically a berry – produced by several kinds
of large herbaceous flowering plants in the genus Musa.

In some countries, bananas used for cooking may be called "plantains",
distinguishing them from dessert bananas. The fruit is variable in size, color,
and firmness, but is usually elongated and curved, with soft flesh rich in
starch covered with a rind, which may be green, yellow, red, purple, or brown
when ripe.
```
Si nos fijamos el layout "post" no existe asi que debemos crearno en el directorio **_layouts** con el nombre de *post.html* y el siguiente contenido:

```html
---
layout: default
---
<h1>{{ page.title }}</h1>
<p>{{ page.date | date_to_string }} - {{ page.author }}</p>

{{ content }}
```
Este codigo es un claro ejemplo de herencia de diseño. 
El diseño de la publicación muestra el título, la fecha, el autor y el cuerpo del contenido que se ajusta con el diseño predeterminado.

También podemos fijarnos en el filtro **date_to_string**, que formatea una fecha en un formato más visible.

**LIST POSTS**

Aun no podemos navegar a la publicación del blog. Por lo general, un blog tiene una página que enumera todas las publicaciones.

Jekyll tiene las publicaciones disponibles en *site.posts*.

Por tanto crearemos **blog.html** en la raíz de nuestro proyecto con el siguiente contenido para poder tener acceso a los posts:

```html
---
layout: default
title: Blog
---
<h1>Latest Posts</h1>

<ul>
  {% for post in site.posts %}
    <li>
      <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
      <p>{{ post.excerpt }}</p>
    </li>
  {% endfor %}
</ul>
```

Cosas a tener en cuenta en el anterior código:

- post.url -> Jekyll establece automáticamente la ruta de salida de la publicación
- post.title -> Se extrae del nombre de archivo de la publicación
- post.excerpt -> Es el primer párrafo de contenido por defecto

Sin embargo, aun nos falta una forma de poder navegar a traves de la barra de navegacion, asi que añadiremos lo siguiente en nuestro archivo *_data/navigation.yml*:

```YML
- name: Blog
  link: /blog.html
```


**MORE POSTS**

Por ahora solo tenemos un post creado. Para enriquecer nuestro sitio vamos a crear algunos mas.

*2018-08-21-apples.md*

```
---
layout: post
author: jill
---
An apple is a sweet, edible fruit produced by an apple tree.

Apple trees are cultivated worldwide, and are the most widely grown species in
the genus Malus. The tree originated in Central Asia, where its wild ancestor,
Malus sieversii, is still found today. Apples have been grown for thousands of
years in Asia and Europe, and were brought to North America by European
colonists.
```


*2018-08-22-kiwifruit.md*

```
---
layout: post
author: ted
---
Kiwifruit (often abbreviated as kiwi), or Chinese gooseberry is the edible
berry of several species of woody vines in the genus Actinidia.

The most common cultivar group of kiwifruit is oval, about the size of a large
hen's egg (5–8 cm (2.0–3.1 in) in length and 4.5–5.5 cm (1.8–2.2 in) in
diameter). It has a fibrous, dull greenish-brown skin and bright green or
golden flesh with rows of tiny, black, edible seeds. The fruit has a soft
texture, with a sweet and unique flavor.
```

Ahora podemos ir a nuestra ruta *localhost:4000/blog.html* y ver nuestros enlaces a los **posts** que hemos creado:

<img  src = "/img/blog.png" alt ="Blog" />

##### Collections <a name="id1.1.9"></a>

En esta sección haremos que cada autor tenga su propia página con una propaganda y las publicaciones que han publicado. Para ello usaremos las **colecciones**, contenido similiar a una publicacion pero sin tener que estar agrupadas por fecha.


**CONFIGURATION**

Para configurar una colección tenemso que decirselo a **Jekyll** y esto se hace un archivo llamado *_config.yml*(por defecto).

Crearemos *_config.yml* en la raíz del proyecto con lo siguiente:

```
collections:
  authors:
```

*Es necesario reiniciar el servidor jekyll.*

**ADD AUTHORS**

Los elementos de una colección estan en una carpeta en la raíz del proyecto de la forma -> *_collection_name*, en nuestro caso se llamará **_authors**.

Crearemos un documento para cada autor:

*_authors/jill.md:*
```
---
short_name: jill
name: Jill Smith
position: Chief Editor
---
Jill is an avid fruit grower based in the south of France.
```

*_authors/ted.md*
```
---
short_name: ted
name: Ted Doe
position: Writer
---
Ted has been eating fruit since he was baby.
```

**STAFF PAGE**


Ahora añadiremos una página que enumera todos los autores en el sitio web. Jekyll permite acceder a la colección en **site.authors**.

Crearemos **staff.html** e iteraremos *site.authors* para generar todos los autores:

```html
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
  {% for author in site.authors %}
    <li>
      <h2>{{ author.name }}</h2>
      <h3>{{ author.position }}</h3>
      <p>{{ author.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>
```

Ahora necesitamos una manera de navegar a esta página a través de la navegación principal.Por ello, añadiremos a nuestro fichero *_data/navigation.yml* lo siguiente:

```YML
- name: Staff
  link: /staff.html
```

***OUTPUT A PAGE**

Las colecciones no generan una página para documentos por defecto y nosotros queremos que cada autor tenga su propia página, así que tenemos que modificar la configuración de la colección.

Para ello añadiremos a **_config.yml** `output: true` a la configuración de la colección de autores. El archivo quedaría de la siguiente forma:

```YML
collections:
  authors:
    output: true
```

*Podemos vinvular la página utilizando author.url*

Añadiremos el enlace a la página staff.html

```HTML
---
layout: default
title: Staff
---
<h1>Staff</h1>

<ul>
  {% for author in site.authors %}
    <li>
      <h2><a href="{{ author.url }}">{{ author.name }}</a></h2>
      <h3>{{ author.position }}</h3>
      <p>{{ author.content | markdownify }}</p>
    </li>
  {% endfor %}
</ul>
```

Una vez que tenemos los enlaces a cada autor tenemos que hacer un diseño para los autores en *_layouts/author.html*:

```html
---
layout: default
---
<h1>{{ page.name }}</h1>
<h2>{{ page.position }}</h2>

{{ content }}
```

**FRONT MATTER DEFAULTS**

Ahora falta configurar los documentos de autor para usar el diseño *author*. Lo que realmente desea es que todas las publicaciones tengan automáticamente el diseño de la publicación.

Puede lograr estoutilizaremos los valores predeterminados del front matter  para añadirlos en *_config.yml*. Estableceremos un ámbito de aplicación a lo que se aplica el valor predeterminadoy posteriormente el asunto que se quiere añadir.

Lo hacemos de la siguiente forma:

```YML
defaults:
  - scope:
      path: ""
      type: "authors"
    values:
      layout: "author"
  - scope:
      path: ""
      type: "posts"
    values:
      layout: "post"
  - scope:
      path: ""
    values:
      layout: "default"
```

Ahora podemos eliminar el layout del *front matter* de todas las páginas y publicaciones, posteriormente reiniciamos el server.


**LIST AUTHOR'S POSTS**
**LINK TO AUTHORS PAGE**





### Despliegue en GitHub Pages <a name="id2"></a>

### Navigation <a name="id3"></a>

### 404 <a name="id4"></a>

### Themes <a name="id5"></a>

### Test the Deployment with html-profer and Travis <a name="id6"></a>

### CI with Travis <a name="id7"></a>

### Despliegues de Jekyll en Netlify <a name="id8"></a>





