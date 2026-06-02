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
