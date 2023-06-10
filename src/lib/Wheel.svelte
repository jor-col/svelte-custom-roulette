<script>
  import Pointer from "./Pointer.svelte";
  import { onMount, tick } from "svelte";
  import { select, arc, pie } from "d3";

  const generateColors = () =>
    `rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(
      Math.random() * 255
    )}, ${Math.floor(Math.random() * 255)})`;

  const spinWheel = () => {
    // while (spinDeg - prevSpin < 2) {
    //   spinDeg = Math.random() * 5; /* change to smaller num for turn 0.x */
    //   console.log("RESPIN", spinDeg - prevSpin);
    // }
    spinDeg += Math.random() * 8 + 3;
    console.log(spinDeg);
    // prevSpin = spinDeg;
  };

  /**
   * @description function to get items[index] based off selected wheel segment
   */

  const getSelectedPrize = () => {
    let startAngle = (this.startRotate * 180) / Math.PI,
      awardAngle = (this.awardRotate * 180) / Math.PI,
      pointerAngle = 90,
      overAngle = (startAngle + pointerAngle) % 360,
      restAngle = 360 - overAngle,
      index = Math.floor(restAngle / awardAngle);

    return items[index];
  };

  let prevSpin = 0;
  // passed down props

  export let size = 400;
  export let pointerColor = "black";
  export let pointerSize = size / 8;
  export let pointerTextColor = "white";
  export let items = ["yes", "no", "maybe"];
  export let colors = Array.from({ length: items.length }, generateColors);
  export let textColor = "white";

  $: spinDeg = 1 / items.length;

  /* wheel sizes */
  const radius = Math.min(size, size) / 2;

  const svgRender = async () => {
    await tick();
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
      .attr("fill", textColor)
      // .style("writing-mode", "vertical-rl")
      // .style("text-orientation", "upright");
      .style("rotate", (_, i) => `${(360 / items.length) * i}deg`); // Math.PI ?
  };

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
    aspect-ratio: 1;
    top: 50%;
    left: 50%;
    background-color: transparent;
    color: white;
    border: none;
    cursor: pointer;
  }
</style>
