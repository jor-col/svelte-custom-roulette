<script>
  import Pointer from './Pointer.svelte'
  import { onMount } from "svelte";
  import { select, arc, pie } from "d3";
  export let pointer = 'green'
  export let items = ["yes", "no", "maybe", 'test', 'test' , 'test', 'test', 'test'];
  const generateColors = () => `rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)}, ${Math.floor(Math.random() * 255)})`
  export let colors = Array.from({length: items.length}, generateColors)
  const spinDuration = 4500;
  
  /* wheel sizes */
  const size = 400;
  const radius = Math.min(size, size) / 2;
  const spinDeg = items.length * 10
  const spinWheel = () => {
    setTimeout(() => {}, spinDuration);
  };

  onMount(() => {
    const svg = select(".wheel").append("svg").attr("width", size).attr("height", size).append("g").attr("transform", `translate(${size / 2}, ${size / 2})`);
    const pieGenerator = pie().value(1);
    const dataWithArc = pieGenerator(items);
    const arcGenerator = arc().innerRadius(0).outerRadius(radius);
    svg.selectAll("path").data(dataWithArc).enter().append("path").attr("d", arcGenerator).attr("fill", (d, i) => colors[i]);
  });
</script>


<div class="wheel-container" id="wheel-container">
  <div style="width .35s ease-in-out; transform: rotate({spinDeg}deg)" class="wheel" />
  <Pointer {pointer}/>
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
    clip-path: circle();    
  }

  .spin-button {
    position: absolute;
    top: 75%;
    left: 49%;
  }

</style>
