## Dynamic pages

Para hacer rutas dinámicas debemos hacer un listado asi

`src > pages > pokemons > [name].astro`

Y va entre corchetes el argumento que le mandamos
Para visitar esa pagina hacemos esto

```astro
---
const { name } = Astro.props;
---

<a href={`/pokemons/${name}`}></a>
```

Si hacemos server side no ocupamos nada mas, pero si hacemos contenido estático ocupamos `getStaticPaths()`, ya que aquí definimos cada pagina

```astro
---
export const getStaticPaths = (() => {
  return [
    {
      params: { name: "bulbasaur" },
      props: { url: "https://..." },
    },
    {
      params: { name: "charmander" },
      props: { url: "https://..." },
    },
  ];
}) satisfies GetStaticPaths;

const { name } = Astro.params;
const { url } = Astro.props;
---
```

Y luego podemos acceder a las params o sus valores
Y asi estas pantallas las genera de forma estática en el build

Y de la misma forma, podemos recibir y usar props

## Dynamic props

Dos cosas importantes

> [!IMPORTANT]
> Podemos poner las clases de tailwind en los styles asi

```astro
<style>
  @reference "../../styles/global.css"

  a {
    @apply hover:underline text-blue-500;
  }
</style>
```

para hacer eso ocupamos importar ya sea el "tailwindcss", o el lugar donde se esta usando

y ya luego usamos apply para poner los esilos

> [!IMPORTANT]
> Podemos reproducir audios con esta etiqueta

```astro
---
const audioSrc = `https://raw.githubusercontent.com/PokeAPI/cries/main/cries/pokemon/latest/${id}.ogg`;
---

<audio controls class="mt-5">
  <source src={audioSrc} type="audio/ogg" />
  Your browser does not support the audio element.
</audio>
```

- controls => sirve para controlar el volumen
- src => el enlace

Y con un mensaje en caso el navegador no soporte el audio

## Create static pages

- SSG => Static side generation
  Cuando le da click a una pagina, debe estar previamente generada para poderla ver

para crear estas paginas en base a todos los Pokemon, consultamos la API y hacemos el map dinámico

```astro
---
export const getStaticPaths = (async () => {
  const resp = await fetch("https://pokeapi.co/api/v2/pokemon?limit=151");
  const { results } = (await resp.json()) as PokemonListResponse;

  return results.map(({ name, url }) => ({
    params: { name },
    props: { name, url },
  }));
}) satisfies GetStaticPaths;
---
```

Y esto en el build generara las 151 paginas

## Conditional Style

podemos aplicar clases condicionales de la forma tradicional con `{isBig && "estilo"}`

O podemos hacer esto

```astro
---
const { isBig = false } = Astro.props;
---

<a
  class:list={[
    `rounded flex flex-col justify-center items-center p-2`,
    {
      border: !isBig,
      "text-4xl text-blue-300": isBig,
    },
  ]}></a>
```

En el array le mandamos los estilos normales, y en el segundo argumento las props que queremos que sean condicionales

En este caso, border se pondrá si !isBig es true, o sea falso

## ViewTransition + Name Transition

También podemos usar transiciones del `ClientRouter`

```astro
<head>
  <ClientRouter />
</head>
```

Y también, se pueden hacer transiciones para elementos en especifico, por ejemplo

```astro
---
const { name, url, isBig = false } = Astro.props;
const imageSrc = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/official-artwork/${id}.png`;
---

<img
  transition:name={`${name}-image`}
  class:list={[
    {
      "w-26 h-26": isBig,
      "w-22 h-22": !isBig,
    },
  ]}
  src={imageSrc}
  alt={name}
/>
```

Y cada elemento debe tener un identificador único, sino astro no va a poder reconocer cual es

## Static pagination

Astro nos da su propia function llamada `paginate()`, que la podemos reutilizar, podemos hacerlo con rutas estáticas `[page].astro` o rutas dinámicas `[...page].astro`

`[...page].astro` => es como decir, todas las paginas que caigan en este componente

> [!IMPORTANT]
> Para mas detalles, ver [[page].astro](../pokemon-static/src/pages/pokemons/[page].astro)

si nos preguntamos, como es que astro no se confunde al poner "pokemons/1" o "pokemons/ivysaur", al hacerlo estatico, imprime todas las posibilidades en base a si es numérico o string

## TypeScript - Path Alias - Dynamic imports

Para hacer importaciones relativas, hay que irse a [tsconfig.json](../pokemon-static/tsconfig.json)

```
{
  compilerOptions: {
    "baseUrl": ".";
    "paths": {
      "@components/*": ["./src/components/*"],
      "@/*": ["src/*"]
    };
  }
}
```

Comenzamos con `baseUrl`, que es donde inicialmente se buscan los archivos, luego en `paths`, ponemos el nombre de la prop, y dentro de una array su equivalente

```astro
---
// import PokemonCard from "../../components/pokemons/PokemonCard.astro";
//* path Alias
import PokemonCard from "@components/pokemons/PokemonCard.astro";
---
```

## Metadatos - Image y descripción

Estas son etiquetas <meta> que van en el <head> del HTML, y sirven para que otros sistemas (buscadores, redes sociales) sepan qué es la página sin tener que "leer" todo el contenido.

SEO:

- description — el resumen corto que Google muestra debajo del título en los resultados de búsqueda. Ayuda a que la gente entienda de qué trata la página antes de hacer clic.
- author — simplemente declara quién hizo la página (metadato informativo, casi no afecta SEO hoy en día).

OG (Open Graph):
Es un estándar creado por Facebook que usan la mayoría de redes sociales (Facebook, LinkedIn, WhatsApp, etc.) para generar la "tarjeta de vista previa" cuando alguien comparte tu link.

- og:title — el título que aparece en esa tarjeta de preview.
- og:description — el texto descriptivo debajo del título en la preview.
- og:image — la imagen que se muestra en la tarjeta (por eso cuando compartes un link en WhatsApp aparece una imagen bonita en vez de solo texto).

En resumen: sin estas etiquetas, si alguien comparte el link de Agritech en redes sociales, se vería como un link pelado sin imagen ni descripción — con ellas, se ve profesional y con contexto.

Usamos esto para el SEO

## Zero JavaScript - ViewTransitions

```astro
<style>
  @view-transition {
    navigation: auto; /* enabled! */
  }
</style>
```

## Indicador de ruta activa

Para obtener la url actual en Astro se usa

```astro
---
const { pathname } = Astro.url;
---
```

### Transition name

podemos usar esta directiva para animar un elemento en especifico, y asi identificar cual es el que va a cambiar de estilos

```astro
{
  pathname === href ? (
    <div transition:name="menu-line" class="border-b-2 border-blue-500 mx-4" />
  ) : (
    <div class="border-b-2 border-transparent mx-4" />
  )
}
```
