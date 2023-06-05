<script>
  import Pointer from "./Pointer.svelte";
  import { onMount } from "svelte";
  import { select, arc, pie } from "d3";

  const generateColors = () =>
    `rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(
      Math.random() * 255
    )}, ${Math.floor(Math.random() * 255)})`;

  // passed down props
  export let items = ["yes", "no", "maybe"];
  export let colors = Array.from({ length: items.length }, generateColors);

  export let pointer = "black";
  export let size = 400;
  export let pointerSize = size / 8;

  export let textColor = "black";
  // let i = 0;
  // let lastIdx = colors.length - 1;

  // attempt at non-repeating color patterns
  // if (items.length > colors.length) {
  //   while (colors.length < items.length) {
  //     console.log("I:", i);
  //     console.log("COLORS[0]:", colors[0], "COLORS[I]:", colors[i]);
  //     console.log(colors.length - 1);
  //     if (colors[i - 1] === colors[i] || colors[i + 1] === colors[i]) {
  //       console.log("!!!!!!!!");
  //       colors.push(colors[i + 2]);
  //     } else if (colors[i] === colors[0] && i + lastIdx === colors.length - 1) {
  //       console.log(Math.floor(colors.length / 2));
  //       colors.push(colors[Math.floor(colors.length / 2)]);
  //     } else {
  //       console.log(colors[i]);
  //       colors.push(colors[i]);
  //     }
  //     ++i;
  //   }
  // }

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
      .selectAll("path")
      .data(dataWithArc)
      .enter()
      .append("path")
      .attr("d", arcGenerator)
      .attr(
        "fill",
        (_, i, arr) =>
          // colors[i] !== undefined ? colors[i] : colors[arr.length % i]
          colors[i]
      )
      .append("text")
      .text((_, i) => items[i])
      .attr("x = 50 y= 50")
      .attr(`color = ${textColor}`);
  });
</script>

<div class="wheel-container" id="wheel-container">
  <div style="transform: rotate({spinDeg}deg)" class="wheel" />
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
