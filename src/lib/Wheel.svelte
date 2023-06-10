<script>
  import Pointer from "./Pointer.svelte";
  import { onMount, afterUpdate } from "svelte";
  import { select, arc, pie } from "d3";

  const generateColors = () =>
    `rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(
      Math.random() * 255
    )}, ${Math.floor(Math.random() * 255)})`;

  // passed down props

  export let pointerColor = "green";
  export let label = ["yes", "no", "maybe"];
  export let colors = Array.from({ length: label.length }, generateColors);
  export let size = 400;
  export let pointerSize = size / 8;
  export let textColor = "white";

  $: spinDeg = 360 / label.length;

  /* wheel sizes */
  const radius = Math.min(size, size) / 2;

  const spinWheel = () => {
    spinDeg = Math.floor(Math.random() * (10000 - 800) + 800);
  };
  let items = [...label]
	afterUpdate(() => {
		if (items !== label)svgRender()
	});
  const svgRender = () => {
    
    select('.wheel svg').remove()
    
    const svg = select(".wheel")
      .append("svg")
      .attr("width", size)
      .attr("height", size)
      .append("g")
      .attr("transform", `translate(${size / 2}, ${size / 2})`);
    const pieGenerator = pie().value(1);
    const dataWithArc = pieGenerator(label);
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
      .text((_, i) => label[i])
      .attr("transform", (d) => `translate(${arcGenerator.centroid(d)})`)
      .style("font-size", 17)
      .attr("fill", textColor)
      //.style("rotate", (_, i) => `${(360 / label.length) * i}deg`);
  };

  // onMount(svgRender);
</script>

<div class="wheel-container" id="wheel-container">
  <div style="rotate :{spinDeg}deg" class="wheel" />
  <Pointer {pointerColor} {pointerSize} />
  <button class="spin-button" on:click={spinWheel}>Spin!</button>
</div>

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
    border-radius: 50%;
    translate: -50% -50%;
    aspect-ratio: 1/1;
    top: 50%;
    left: 50%;
    background-color: transparent;
    border: none;
    cursor: pointer;
  }
</style>
