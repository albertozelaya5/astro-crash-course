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

