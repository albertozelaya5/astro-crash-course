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
