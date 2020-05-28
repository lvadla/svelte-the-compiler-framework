<script>
  import { getContext } from "svelte";
  import { ctx } from "./App.svelte";
  const { addSlide, currentSlide } = getContext(ctx);

  // every Slide can receive styles i.e. background-image
  export let style = "";
  // every Slide can have multiple steps (set through props)
  export let steps = 1;
  // every Slide keeps track of their current step locally
  let step = 0;
  $: console.log(step);

  const slide = {
    steps,
    hasANextStep: () => {
      if (step >= steps - 1) {
        return false;
      }
      return true;
    },
    hasAPrevStep: () => {
      if (step <= 0) {
        return false;
      }
      return true;
    },
    // determined by parent
    nextSlide: null,
    // increase step so slide instances show the next scene
    nextStep: () => {
      step += 1;
    },
    // determined by parent
    prevSlide: null,
    // decrease step so slide instances show the previous scene
    prevStep: () => {
      step -= 1;
    },
    resetStep(isLastStep) {
      step = isLastStep ? steps - 1 : 0;
    }
  };

  addSlide(slide);
</script>

<style>
  div.slide {
    background: 50% 50% no-repeat;
    background-size: cover;
    box-sizing: border-box;
    display: flex;
    flex-direction: column;
    height: 100%;
    padding: 2vw;
    position: absolute;
    width: 100%;
  }
</style>

{#if $currentSlide === slide}
  <div class="slide" {style}>
    <slot {step} />
  </div>
{/if}
