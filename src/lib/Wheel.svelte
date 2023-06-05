<script>
  import Pointer from "./Pointer.svelte";
  import { onMount } from "svelte";
  import { select, arc, pie } from "d3";

  const generateColors = () =>
    `rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(
      Math.random() * 255
    )}, ${Math.floor(Math.random() * 255)})`;

  // passed down props
  export let pointer = "black";
  export let items = ["yes", "no", "maybe"];
  export let colors = Array.from({ length: items.length }, generateColors);
  export let size = 400;
  export let pointerSize = size / 8;
  export let textColor = "white";

  $: spinDeg = 360 / items.length;

  /* wheel sizes */
  const radius = Math.min(size, size) / 2;

  const spinWheel = () => {
    spinDeg = Math.floor(Math.random() * (10000 - 800) + 800);
  };

  onMount(() => {
    const svg = select(".wheel")
      .append("svg")
      .attr("width", size)
      .attr("height", size)
      .append("g")
      .attr("transform", `translate(${size / 2}, ${size / 2})`);
    const pieGenerator = pie().value(1);
    const dataWithArc = pieGenerator(items);
    const arcGenerator = arc().innerRadius(0).outerRadius(radius);
    svg
      .selectAll("mySlices")
      .data(dataWithArc)
      .enter()
      .append("path")
      .attr("d", arcGenerator)
      .attr("fill", (d, i) => colors[i]);
    svg
      .selectAll("mySlices")
      .data(dataWithArc)
      .enter()
      .append("text")
      .text((_, i) => items[i])
      .attr("transform", (d) => `translate(${arcGenerator.centroid(d)})`)
      .style("font-size", 17)
      .style("fill", textColor)
      .style("rotate", (_, i) => `${(360 / items.length) * i}turn`);
  });
</script>

<div class="wheel-container" id="wheel-container">
  <div style="rotate :{spinDeg}deg" class="wheel" />
  <Pointer {pointer} {pointerSize} />
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

  .wheel {
    clip-path: circle(45%);
    transition: 2s ease-in-out;
  }

  .spin-button {
    position: absolute;
    top: 75%;
    left: 49%;
  }
</style>
