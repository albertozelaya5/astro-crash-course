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
