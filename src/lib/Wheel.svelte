<script>
  import Segment from "./Segment.svelte";
  export let items = ["yes", "no"];
  let deg = 360 / items.length;
</script>

<div class="wheel-container">
  <div class="wheel">
    {#each items as item, index}
      <Segment {item} {deg} {index} />
    {/each}
  </div>
  <img class="pointer" src="src/lib/roulette-pointer.png" alt="pointer" />
</div>

<style>
  .wheel-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
  }

  .wheel {
    background: linear-gradient(#e66465, #9198e5);
    width: 50%;
    aspect-ratio: 1/1;
    clip-path: circle();
    animation: spin 1.3s linear 2;
  }

  .pointer {
    position: absolute;
    top: 50%;
    left: 48.7%;
    transform: rotate(135deg);
    height: 50px;
    width: 50px;
  }

  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
</style>
