<script>
  import Slide from "../Slide.svelte";

  import { fade } from "svelte/transition";
</script>

<style>
  main {
    /* TODO - move this to slide 1 */
    text-align: center;
    margin: 0 auto;
  }

  h1 {
    /* TODO - move this to slide 1 */
    color: var(--primary);
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 400;
  }

  img {
    position: absolute;
  }

  img.logo {
    left: 60%;
    top: 36.7%;
    width: auto;
    transform: translate(-50%, -50%);
  }
</style>

<Slide
  style="background-image: url(images/leaves-mark-hang-fung-so-unsplash.jpg);"
  steps={1}
  let:step>
  {#if step === 0}
    <div
      style="width: 60vw; text-align: center; font-size: 2vw;"
      transition:fade|local>
      <p>Press right arrow to start &rarr;</p>
      <p>Press left arrow to go back a slide &larr;</p>
    </div>
  {/if}

  {#if step >= 1}
    <div style="will-change: opacity;" in:fade={{ duration: 2000 }}>
      <img class="logo" alt="projekt202" src="images/projekt202.png" />
    </div>
  {/if}
</Slide>
