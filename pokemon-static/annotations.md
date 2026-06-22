## Peticiones HTTP en tiempo de construcción

Astro no ocupa list porque es js estático, por lo que no ocupa recordar las posiciones

```astro
{data.results.map(({ name, url }) => <PokemonCard name={name} url={url} />)}
```
