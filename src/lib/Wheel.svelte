<script>
  import Segment from "./Segment.svelte";
  import { onMount } from "svelte";
  import { select, arc, pie } from "d3";
  export let items = ["yes", "no", "maybe"];
  let deg = 360 / items.length;

  onMount(() => {
    const width = 400;
    const height = 400;
    const radius = Math.min(width, height) / 2;

    const svg = select("#wheel")
      .append("svg")
      .attr("width", width)
      .attr("height", height)
      .append("g")
      .attr("transform", `translate(${width / 2}, ${height / 2})`);

    const color = ["#ff7f0e", "#2ca02c", "#9467bd"];

    const pieGenerator = pie().value(1);
    const dataWithArc = pieGenerator(items);

    const arcGenerator = arc().innerRadius(0).outerRadius(radius);

    svg
      .selectAll("path")
      .data(dataWithArc)
      .enter()
      .append("path")
      .attr("d", arcGenerator)
      .attr("fill", (d, i) => color[i]);
  });
</script>

<div class="wheel-container">
  <!-- <div class="wheel">
    {#each items as item, index}
      <Segment {item} {deg} {index} />
    {/each}
  </div> -->
  <div id="wheel" />
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
