# Optional: Make a content collection

Ahora que ya tenemos hecha nuestra Astro site `built-in-file-based routing`, la vamos a actualizar para usar un `content collection`

`Content collection` son bastante poderosos para gestionar grupos similares de contenido, como blog posts

- Move your folder of blog posts into src/blog/
- Create a schema to define your blog post frontmatter
- Use getCollection() to get blog post content and metadata

Siempre se pueden usar los post, pero al moverlos afuera, se podrán usar APIS mas potentes y con mas rendimiento

Y también tendremos acceso a una librería de validaciones => `Zod`
Ya que tendremos un esquema para describir mejor la estructura de cada post

## Convertir posts a blogs

Ahora vamos a actualizar todas las integraciones a su ultima version corriendo el comando

```bash
# Upgrade Astro and official integrations together
pnpm dlx @astrojs/upgrade
```

Vamos a crear un archivo de configuration `content.config.ts`, para definir el schema de las `postCollection`

Para que astro reconozca este esquema, hay que quitar `ctrl + c` y volver a correr `pnpm dev`, para definir el `astro:content` module

## Generate pages from a collection

Crearemos `[...slug].astro`, nuestros archivos .md y MDX no se van a convertir automáticamente en paginas cuando se usa el Astro file-based routing cuando están dentro de una collection

Entonces debemos crear una pagina responsable de generar cada post

Todos los lugares donde tenemos `import.meta.glob()` los vamos a reemplazar con `getCollection()`, como la forma de obtener contenido y la metadata de nuestros markdowns

```astro
---
import { getCollection } from "astro:content";
// const allPosts = Object.values(
//   import.meta.glob("./posts/*.md", { eager: true }),
// );
const allPosts = await getCollection("blog");
---
```

También, todo lo que estaba guardado en `frontmatter` ahora lo estará en `data`, y por cada pagina habrá una ruta de su id, en lugar de poner la ruta completa

```astro
{
  allPosts.map((post: any) => (
    // <BlogPost url={post.url} title={post.frontmatter.title} />
    <BlogPost url={`/posts/${post.id}`} title={post.data.title} />
  ))
}
```
