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


### Despliegue en GitHub Pages <a name="id2"></a>

### Navigation <a name="id3"></a>

### 404 <a name="id4"></a>

### Themes <a name="id5"></a>

### Test the Deployment with html-profer and Travis <a name="id6"></a>

### CI with Travis <a name="id7"></a>

### Despliegues de Jekyll en Netlify <a name="id8"></a>





