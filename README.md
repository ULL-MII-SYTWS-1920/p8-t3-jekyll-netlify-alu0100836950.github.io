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

<img  src = "/img/highlighting.png" alt ="Page highlighting" />

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
##### Collections <a name="id1.1.9"></a>






### Despliegue en GitHub Pages <a name="id2"></a>

### Navigation <a name="id3"></a>

### 404 <a name="id4"></a>

### Themes <a name="id5"></a>

### Test the Deployment with html-profer and Travis <a name="id6"></a>

### CI with Travis <a name="id7"></a>

### Despliegues de Jekyll en Netlify <a name="id8"></a>





