# Build your first Astro Blog

## Unidad 1

Vamos a hacer un blog, donde aprenderemos esto:

- Set up your development environment
- Create pages and blog posts for your website
- Build with Astro components
- Query and work with local files
- Add interactivity to your site
- Deploy your site to the web

Le damos a

```bash
pnpm create astro@latest
```

Uno nombra la carpeta donde se va a aguardar el proyecto

El index del proyecto esta en `src > pages > index.astro`, y ahi se puede editar como si fuera `index.html`

### Instalar Prettier

Para prettier es con

```bash
pnpm add --save-dev --save-exact prettier prettier-plugin-astro
```

Y en la ruta superior crear un `.prettierrc`, copiar y pegar lo que da la documentation

### Instalar Eslint

````bash
pnpm add --save-dev eslint eslint-plugin-astro
```

```bash
pnpm install --save-dev @typescript-eslint/parser
````

> [!NOTE]
> Es opcional subir el sitio a netlify

## Unidad 2

Nos vamos a encargar de esto

- Create your first Astro pages with the .astro syntax
- Add blog posts with Markdown (.md) files
- Style an individual page with <style>
- Apply global styles across pages

Along the way, you’ll learn how the two sections of a .astro file work together to create a page, and how to use variables and conditional rendering on your pages.

Los `.astro` files son los responsables para las paginas en nuestro sitio

- Create two new pages on your website: About and Blog
- Add navigation links to your pages
- Deploy an updated version of your website to the web

Luego para crear una pagina o ruta, en el mismo `src > pages` folder donde esta el index.astro, creamos `about.astro`, que sera el nombre de la ruta donde nos vamos a dirigir

Asi en nuestro navegador, cuando pongamos "/about" ademas de la ruta normal, nos mandara a ese archivo, donde tenemos otro html, y podemos cambiarlo a como se nos antoje, etc

También podemos agregar rutas, aquí se usan los `a` de toda la vida, ya que es un SSR site

### Write your first Markdown blog post

Uno también puede crear paginas con un `.md`, por ejemplo `/posts/post-1.md`

Ahi podemos diferenciar en las rutas, al ponerlas en el navegador, cuando existe la pagina y cuando no, por ejemplo poner `/posts/post-2.md` aunque no este creado, para ver el error

Este formato que luce asi

```markdown
---
title: "My First Blog Post"
---
```

Se llama `frontmatter`, es information sobre nuestro post que Astro puede usar, aunque no aparece en la pagina automáticamente

También, podemos poner un anchor en nuestro `blog.astro`, para que nos dirija a nuestro post, asi

```astro
<ul>
  <li><a href="/posts/post-1/">Post 1</a></li>
</ul>
```

> [!TIP]
> Recordar que ahi los anchor se colocan asi

```markdown
[Discord community](https://astro.build/chat)
```

### Add dynamic content about you

EN los .astro podemos hacer mas que poner un html

Podemos hacer js también, y para eso usamos esto `---`

```astro
---
const pageTitle = "My Astro Learning Blog";
---

<html lang="en">
  <!-- RESTO DEL CONTENIDO -->
  <title>{pageTitle}</title>
</html>
```

Asi le pasamos data dinámica al html, pudiendo modificar la metadata también!

> [!IMPORTANT]
> Entre estas `---` frontmatter script, solo contiene javascript
> Y para incluir ese js en el html se ocupa de esto `{}`

```astro
---
const skills = ["HTML", "CSS", "JavaScript", "React", "Astro", "Writing Docs"];
const goal = 3;
---

<ul>{skills.map((skill) => <li>{skill}</li>)}</ul>

{
  goal === 3 ? (
    <p>My goal is to finish in 3 days</p>
  ) : (
    <p>My goal is not 3 days.</p>
  )
}
```

### Style your About page

Ademas de js, tambien podemos poner css, y para ello usamos la etiqueta `style`

```astro
<style>
  h1 {
    color: purple;
    font-size: 4rem;
  }
</style>
```

Y asi como en el html, también podemos mandar variables de js, pero para ello, ocupamos mandar una directiva de astro llamada `define:vars`, que es un objeto, donde le pasamos las variables que ocupemos

Como en react al pasar css

```astro
<style define:vars={{ skillColor, fontWeight, textCase }}>
  h1 {
    color: purple;
    font-size: 4rem;
  }

  .skill {
    color: var(--skillColor);
    font-weight: var(--fontWeight);
    text-transform: var(--textCase);
  }
</style>
```

> [!NOTE]
> Todos estos estilos solo aplican al modulo en si, no he visto algo global aun

#### Como el global.css y el <style> tag trabajan juntos?

Cuando hay un conflicto, siempre en el que esta en el style del `.astro` va a tener prioridad

Eso y, también se pueden importar estilos globales, pero solo desde el js

```astro
---
import "../styles/global.css";

const pageTitle = "Home Page";
---
```

### Check in: Unit 3 - Components

Al fin vamos a crear componentes de astro

Vamos a hacer esto

- A Navigation component that presents a menu of links to your pages
- A Footer component to include at the bottom of each page
- A Social Media component, used in the Footer, that links to profile - pages
- An interactive Menu component to toggle the Navigation on mobile

### Make a reusable Navigation component

Hacemos la carpeta de `src > components`

La diferencia entre esto y archivos normales, es la nomenclatura, ya que los componentes siempre inician en PascalCase

Ahora estos componentes pueden tener props, y para ello usamos esto

Este es el componente, para leer las props se leen de `Astro.props`

```astro
---
const { platform, username } = Astro.props;
---

<a href={`https://www.${platform}.com/${username}`}>{platform}</a>
```

Y aquí se le envían las props

```astro
---
import Social from "./Social.astro";
---

<Social platform="youtube" username="astrodotbuild" />
```

> [!NOTE]
> Para estilizar, se puede usar la etiqueta `style`, no importa si es al final del html

```astro
<a href={`https://www.${platform}.com/${username}`}>{platform}</a>

<style>
  a {
    padding: 0.5rem 1rem;
  }
</style>
```

### Build it yourself - Header

Vamos a hacer el sitio responsive, aprenderemos a hacer esto

- Create a Header for your site that contains the Navigation component
- Make the Navigation component responsive

También podemos poner componentes dentro de otros componentes

Y podemos asignarle clases para estilizar globalmente

```astro
<div id="main-menu" class="nav-links">
  <a href="/">Home</a>
</div>
```

En el [global.css](/src/styles/global.css)

```css
@media (min-width: 40em) {
  .nav-links {
    margin-left: 5em;
  }

  .nav-links a {
    display: inline-block;
  }
}
```

### Send your first script to the browser

Haremos un botón para crear y cerrar el menu de navegación

- Create a menu component
- Write a <script> to allow your site visitors to open - and close the navigation menu
- Move your JavaScript to its .js file

Le ponemos un has

```css
:has(.menu[aria-expanded="true"]) .nav-links {
  display: unset;
}
```

A grandes rasgos dice: "Busca un contenedor padre que tenga adentro (:has) un elemento .menu abierto (aria-expanded="true"). Si lo encuentras, aplícate los siguientes estilos a su elemento hijo .nav-links".

Cuando es `unset`, lo que hace es borrar o Resetear el valor que tenga la propiedad antes, usando el valor por defecto

También se puede poner js en los .astro, antes de que termine el `body`

```html
<script>
  const menu = document.querySelector(".menu");

  menu?.addEventListener("click", () => {});
</script>
```

### Check in: Unit 4 - Layouts

Vamos a

- Create reusable layout components
- Pass content to your layouts with <slot />
- Pass data from Markdown frontmatter to your layouts
- Nest multiple layouts

Asi como hicimos componentes, vamos a hacer un `Layout general`, para ello nos vamos a `src > pages > layouts > BaseLayout.astro`

Y ahi ponemos todo lo base que pusimos en el `index.astro`

Ver [BaseLayout](/src/pages/layouts/BaseLayout.astro)

Y esa es la magia, refactorizamos todo lo demás✨

Y lo que sea diferente, lo ponemos dentro del componente, estilos, etc
Y el js lo ponemos siempre arriba

```astro
---
import BaseLayout from "./layouts/BaseLayout.astro";
const happy = true;
---

<BaseLayout pageTitle={pageTitle}>
  <style define:vars={{ skillColor, fontWeight, textCase }}></style>
  //* RESTO DE ESTILOS

  {happy && <p>I am happy to be learning Astro!</p>}
</BaseLayout>
```

Y en el `BaseLayout` component, para que se muestra toda la information de los children, usamos el `slot` placeholder

```astro
---

---

<slot />
<script>
  import "../scripts/menu";
</script>
```

Y asi pasamos código javascript también

### Create and pass data to a custom blog layout

Vamos a poner un layout a nuestro blog

- Create a new blog post layout for your Markdown files
- Pass YAML frontmatter values as props to layout component

Para los markdowns, usamos la propiedad `frontmatter`, lo demas es lo mismo

```astro
---
const { frontmatter } = Astro.props;
---

<meta charset="utf-8" />
<h1>{frontmatter.title}</h1>

<slot />
```

Y en los md esas props se mandan en esto

```markdown
---
layout: ../layouts/MarkdownPostLayout.astro
title: "My First Blog Post"
---
```

E importamos el componente mediante la propiedad `layout`

### Combine layouts to get the best of both worlds

Ahora podemos hacer que estos markdowns tengan el mismo formato que el resto de la pagina, esto con ayuda del `BaseLayout`, que lo pondremos en `MarkdownPostLayout`

```astro
<BaseLayout pageTitle={frontmatter.title}>
  <p>Published on: {frontmatter.pubDate.toString().slice(0, 10)}</p>
  <!-- RESTO DEL COMPONENTE QUE VA EN LOS MARKDOWNS -->
  <slot />
</BaseLayout>
```

## Check in: Unit 5 - Astro API

Vamos a cargar nuestro blog con una `RSS feed` => sepa que es eso xd

- import.meta.glob() to access data from files in your project
- getStaticPaths() to create multiple pages (routes) at once
- The Astro RSS package to create an RSS feed

### Create a blog post archive

En lugar de poner los post de forma estática, los podemos mandar de forma dinámica

```astro
---
const allPosts = Object.values(
  import.meta.glob("./posts/*.md", { eager: true }),
);
---

<ul>
  {
    allPosts.map((post: any) => (
      <BlogPost url={post.url} title={post.frontmatter.title} />
    ))
  }
</ul>
```

El `import.meta.glob()` nos dará un objeto, que tenga las propiedades de todos los post, ocupa dos argumentos

1. La ubicación de el, o los post (poner `*` si quiere que sean todos)
2. un objeto con la prop `eager`, que sirve para pasar de una carga asíncrona(lazy), a síncrona(eager)

Y el `BlogPost` component es para hacer un list de `a`, mandando sus propiedades

### Generate tag pages

Podemos crear infinitas paginas usando `.astro`, que exporta una function `getStaticPaths()`

Para crear rutas dinámicas debemos hacer un archivo con nombre dinámico `[tag].astro`, y este archivo debe exportar la function `getStaticPaths()`

Esta function debe retornar un array anidado con esta estructura

```astro
---
export async function getStaticPaths() {
  const allPosts = Object.values(
    import.meta.glob("../posts/*.md", { eager: true }),
  );

  return [{ params: { tag: "astro" }, props: { posts: allPosts } }];
}
---
```

Ya que queremos mandar una lista de los post, usamos nuevamente el `import.meta.glob()`

Y ese array anidado tendrá objetos que lleven `params`, y ahi definir las propiedades que queramos

Esas propiedades dentro de params las vamos a usar en nuestro código

```astro
---
const { tag } = Astro.params;
const { posts } = Astro.props;
const filteredPosts = posts.filter((post: any) =>
  post.frontmatter.tags?.includes(tag),
);
---
```

Y aquí en base a lo que obtenemos de `allPosts`, filtramos los post por los que incluyen en sus `tags` esas palabras

Ahora todas esta estructura rara, es también porque usamos rutas estáticas

---

Ahora vamos aprender a como generar estas rutas dinámicas, una ruta por cada `tag`

Primero, ocupamos solo los tags, sin repetidos

```astro
---
const allPosts = Object.values(
  import.meta.glob("../posts/*.md", { eager: true }),
);
const uniqueTags = [
  ...new Set(allPosts.map((post: any) => post.frontmatter.tags)),
];
---
```

Esto siempre dentro de la `getStaticPaths()` ya que solo lo ocupamos ahi

Luego, de esos tags únicos, vamos a imprimir una lista de los posts, que tengan incluidos tags que coincidan

Y por cada tag(ruta), vamos a imprimir los posts que incluyan el tag que se este mostrando

En resumen, por cada pagina (tags), se mostaran los posts que tienen tags en común en sus `---` o frontmatter

> [!TIP]
> Aunque le ponga _learning%20in%20public_, lo va a leer como "learning in public"

Y asi, se hace que cada de esas nuevas rutas coincida con los componentes que queremos

### Build a tag index page

- Vamos a agregar una nueva pagina usando un nuevo patron de rutas
- Mostrar una lista de nuestras tags únicas, con enlaces a las paginas de cada tag

En este index, vamos a mostrar una lista de los tags, con las rutas para llegar a ellos, usamos siempre el `allPosts` y el `uniqueTags`

```astro
<div>
  {
    uniqueTags.map((tag) => (
      <p>
        <a href={`/tags/${tag}`}>{tag}</a>
      </p>
    ))
  }
</div>
```

Y gracias al código anterior

Ya luego estilizamos eso

Entonces, tenemos dos formas de generar una ruta, mediante `nombre.astro`, o `nombre/index.astro`

Y al buscar la ruta `/nombre/` saldrá lo mismo

### Add an RSS feed

Un RSS feed es simplemente un archivo de texto automático que contiene la lista de mis posts markdowns con su título, descripción y enlace.

Este documento se actualiza cada que se hace un built

Ahora vamos a

- Instala un paquete Astro para crear un feed RSS para tu sitio web.
- Crea un feed al que se pueda suscribir y que puedan leer los lectores de feeds RSS.

Astro tiene una librería para eso

Usamos

```bash
pnpm add @astrojs/rss
```

