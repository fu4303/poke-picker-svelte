<script>
  import { fade } from "svelte/transition";
  import pokemon from "./data";

  const COLS = 5;
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
  .grid {
    max-width: 22rem;
    display: grid;
    grid-template-columns: repeat(5, 4rem);
    grid-template-rows: repeat(5, 4rem);
    grid-gap: 0.5rem;
  }
  .gridcell {
    display: grid;
    place-content: center;
    border: 0.125rem solid hsl(200, 10%, 80%);
    margin: 0;
    transition: border-color 0.1s, transform 0.1s;
  }

  .gridcell.active,
  .gridcell:focus {
    outline: 0;
    border-color: hsl(20, 80%, 60%);
    transform: scale(1.2) rotate(2deg);
  }

  .gridcell:nth-child(2n).active,
  .gridcell:nth-child(2n):focus {
    transform: scale(1.3) rotate(-2deg);
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
  @media (min-width: 74rem) {
    .layout {
      display: grid;
      grid-template-columns: 1fr 1fr;
      grid-gap: 2rem;
      align-items: start;
      padding: 2rem;
    }
  }
  @media (min-width: 50rem) {
    .grid {
      max-width: 40rem;
      grid-template-columns: repeat(5, 7rem);
      grid-template-rows: repeat(5, 7rem);
      grid-gap: 1rem;
    }
  }
</style>

<svelte:window on:keydown={handleKeyDown} />

<main class="layout">
  <div>
    <h1 id="chooseFighter">Choose your fighter</h1>
    <div class="grid" role="grid" aria-labelledby="chooseFighter">
      {#each pokemon as p, i}
        <figure
          tabindex="-1"
          bind:this={refs[i]}
          on:click={() => {
            x = i % COLS;
            y = Math.floor(i / ROWS);
          }}
          class:active={index === i}
          class="gridcell">
          <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/{p.id}.png" alt={p.name} />
        </figure>
      {/each}
    </div>
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
