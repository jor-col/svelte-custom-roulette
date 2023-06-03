<script>
  import Segment from "./Segment.svelte";
  import { onMount } from "svelte";
  import { select, arc, pie } from "d3";
  export let items = ["yes", "no", "maybe"];
  export const colors = ["#18D2AD", "#8236AD", "#7098E9"];
  const spinDuration = 4500;
  let spin = "";
  /* wheel sizes */
  let deg = 360 / items.length;
  const width = 400;
  const height = 400;
  const radius = Math.min(width, height) / 2;

  const spinWheel = () => {
    spin = "wheelSpin";
    setTimeout(() => {
      spin = "";
    }, spinDuration);
  };

  onMount(() => {
    const svg = select(".wheel")
      .append("svg")
      .attr("width", width)
      .attr("height", height)
      .append("g")
      .attr("transform", `translate(${width / 2}, ${height / 2})`);

    const pieGenerator = pie().value(1);
    const dataWithArc = pieGenerator(items);

    const arcGenerator = arc().innerRadius(0).outerRadius(radius);

    svg
      .selectAll("path")
      .data(dataWithArc)
      .enter()
      .append("path")
      .attr("d", arcGenerator)
      .attr("fill", (d, i) => colors[i]);
  });
</script>

<div class="wheel-container" id="wheel-container">
  <!-- <div class="wheel">
    {#each items as item, index}
      <Segment {item} {deg} {index} />
    {/each}
  </div> -->
  <div class={spin + " wheel"} />
  <img class="pointer" src="src/lib/roulette-pointer.svg" alt="pointer SVG" />
</div>
<button class="spin-button" on:click={spinWheel}>Spin!</button>

<style>
  .wheel-container {
    position: absolute;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
  }

  /* old wheel */
  /* .wheel {
    background: linear-gradient(#e66465, #9198e5);
    width: 50%;
    aspect-ratio: 1/1;
    clip-path: circle();
    animation: spin 1 linear 2;
  } */

  .wheel {
    clip-path: circle();
  }

  .wheelSpin {
    animation: spin 1s linear infinite;
  }

  .pointer {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%) rotate(136deg);
    height: 50px;
    width: 50px;
  }

  .spin-button {
    position: absolute;
    top: 75%;
    left: 49%;
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
