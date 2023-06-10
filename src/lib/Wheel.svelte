<script>
  import Pointer from "./Pointer.svelte";
  import { onMount, beforeUpdate } from "svelte";
  import { select, arc, pie } from "d3";

  const generateColors= ()=>`rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)})`;


  // passed down props
  export let pointerColor = "black";
  export let pointerTextColor = 'white'
  export let items = ["yes", "no", "maybe"];
  export let colors = Array.from({ length: items.length }, generateColors);
  export let size = 400;
  export let pointerSize = size / 8;
  export let textColor = "white";
// label
  $: spinDeg = 360 / items.length;

  /* wheel sizes */
  $: radius = Math.min(size, size) / 2;

  const spinWheel = () => {
    spinDeg += Math.random() * 8 + 3;
  };
  let segmentColors = [...colors]
  
  const colorArrayLengthMatcher =()=>{
    if(colors.length !== items.length){
      segmentColors = Array.from({length: items.length}, (_,i)=>colors[i % colors.length])
    }
  }

  const svgRender = () => {
    select('.wheel svg').remove()
    colorArrayLengthMatcher()
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
      .attr("fill", (d, i) => segmentColors[i]);
    svg
      .selectAll("mySlices")
      .data(dataWithArc)
      .enter()
      .append("text")
      .text((_, i) => items[i])
      .attr("transform", (d) => `translate(${arcGenerator.centroid(d)})`)
      .style("font-size", 17)
      .attr("fill", textColor)
      //.style("rotate", (_, i) => `z ${((360 / items.length) * i )}deg`);
  };
  beforeUpdate(svgRender)
  onMount(svgRender);
</script>

<div class="wheel-container" id="wheel-container">
  <div style="rotate: {spinDeg}turn" class="wheel" />
  <Pointer {pointerColor} {pointerSize} />
  <button class="spin-button" on:click={spinWheel} color={pointerTextColor}
    >Spin!</button
  >
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
    transition: 5s ease-in-out;
  }

  .spin-button {
    position: absolute;
    border-radius: 50%;
    translate: -50% -50%;
    aspect-ratio: 1/1;
    top: 50%;
    left: 50%;
    background-color: transparent;
    color: white;
    border: none;
    cursor: pointer;
  }
</style>
