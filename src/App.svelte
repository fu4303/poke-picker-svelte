<script>
  import { fade } from "svelte/transition";
  import pokemon from "./data";

  const ROWS = 5;

  let x = 0;
  let y = 0;
  let refs = [];

  $: index = x + y * ROWS;
  $: if (refs[index]) refs[index].focus();

  function handleKeyDown(event) {
    if (event.key.includes("Arrow")) event.preventDefault();
    switch (event.key) {
      case "ArrowRight":
        x = Math.min(x + 1, ROWS - 1);
        break;
      case "ArrowLeft":
        x = Math.max(x - 1, 0);
        break;
      case "ArrowDown":
        y = Math.min(y + 1, ROWS - 1);
        break;
      case "ArrowUp":
        y = Math.max(y - 1, 0);
        break;
    }
  }
</script>

<style>
  @media (min-width: 50rem) {
    .layout {
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-gap: 2rem;
      align-items: start;
    }
  }
  .grid {
    width: 95vw;
    max-width: 25rem;
    display: grid;
    grid-template-columns: repeat(5, 5rem);
    grid-template-rows: repeat(5, 5rem);
    border: 1px solid;
  }
  .gridcell {
    display: grid;
    place-content: center;
    border: 1px solid;
    margin: 0;
  }

  .gridcell.active,
  .gridcell:focus {
    background-color: yellow;
  }

  .gridcell > img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .big-sprite {
    margin: 0;
    height: 100%;
    display: grid;
    place-items: center;
  }

  .big-sprite > img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    grid-area: 1/ 1/ 1/ 1;
    opacity: 0.3;
    z-index: -10;
  }
  .big-sprite > figcaption {
    grid-area: 1/ 1/ 1/ 1;
    font-size: 8rem;
    font-weight: 900;

    background-repeat: no-repeat;
    background-size: cover;
    background-position: center center;
    font-size: 8rem;
    font-weight: 900;
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color: hsla(200, 10%, 30%, 0.25);
  }
</style>

<svelte:window on:keydown={handleKeyDown} />

<h1 id="chooseFighter">Choose your fighter</h1>

<main class="layout">
  <div class="grid" role="grid" aria-labelledby="chooseFighter">
    {#each pokemon as p, i}
      <figure
        tabindex="-1"
        bind:this={refs[i]}
        class:active={index === i}
        class="gridcell">
        <figcaption>{p.name}</figcaption>
        <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/{p.id}.png" alt="" />
      </figure>
    {/each}
  </div>

  <!-- <figure class="big-sprite" key={index}>
    <img transition:fade src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/{pokemon[index].id}.png" alt="" />
    <figcaption>{pokemon[index].name}</figcaption>
  </figure> -->
  <figure class="big-sprite">
    <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/{pokemon[index].id}.png" alt="" />
    <figcaption style="background-image:
      url('https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/{pokemon[index].id}.png')">{pokemon[index].name}</figcaption>
  </figure>

</main>
