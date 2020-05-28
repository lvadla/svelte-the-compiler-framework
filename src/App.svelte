<script context="module">
  export const ctx = {};
</script>

<script>
  import Welcome from "./Slides/Welcome.svelte";

  import { setContext } from "svelte";
  import { writable } from "svelte/store";
  const currentSlide = writable(null);

  let slides = [];
  let lastSlideShown;

  setContext(ctx, {
    currentSlide,
    addSlide(slide) {
      // the new slide will be pushed to the end of the
      // slides array so update the index based off length
      slide.index = slides.length;

      // if this is not the first slide shown
      if (lastSlideShown) {
        // point the last slide to this slide
        lastSlideShown.nextSlide = slide;
        // point this slide to the last slide
        slide.prevSlide = lastSlideShown;
      }

      // base case to grab the first slide
      if (slide.index === 0) {
        $currentSlide = slide;
      }

      slides.push(slide);
      lastSlideShown = slide;
    }
  });

  function navigate(event) {
    switch (event.keyCode) {
      case 39: // the right arrow key
        if ($currentSlide.hasANextStep) {
          $currentSlide.nextStep();
          return;
        }
        if ($currentSlide.nextSlide) {
          $currentSlide = $currentSlide.nextSlide;
          // we want to go forward to the first
          // step of the next slide
          const isLastStep = false;
          $currentSlide.resetStep(isLastStep);
        }
        break;
      case 37: // the left arrow key
        if ($currentSlide.hasAPrevStep) {
          $currentSlide.prevStep();
          return;
        }
        if ($currentSlide.prevSlide) {
          $currentSlide = $currentSlide.prevSlide;
          // we want to go back to the last step
          // of the previous slide
          const isLastStep = true;
          $currentSlide.resetStep(isLastStep);
        }
        break;
      default:
    }
  }

  $: if ($currentSlide !== null) {
    window.location.hash = `${$currentSlide.index + 1}`;
  }
</script>

<style>
  .presentation {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
  }
</style>

<svelte:window on:keydown={navigate} />

<div class="presentation">
  <Welcome />
</div>
