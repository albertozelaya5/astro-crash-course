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
    },
    {
      params: { name: "charmander" },
    },
  ];
}) satisfies GetStaticPaths;

const { name } = Astro.params;
---
```

Y luego podemos acceder a las params o sus valores
Y asi estas pantallas las genera de forma estática en el build
