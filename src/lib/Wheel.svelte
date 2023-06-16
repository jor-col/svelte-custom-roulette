<script>
  import Pointer from "./Pointer.svelte";
  import { onMount, beforeUpdate } from "svelte";
  import { select, arc, pie } from "d3";

  let isSpinning = false;
  let spinDeg = 360;

  const generateColors = () =>
    `rgb(${Math.floor(Math.random() * 255)}, ${Math.floor(
      Math.random() * 255
    )}, ${Math.floor(Math.random() * 255)})`;

  /**
   * @description function from React Turntable to get item[index] from "pointer" svg segment
   * @link https://github.com/lijinke666/react-turntable/blob/master/src/index.js
   */
  //   getSelectedPrize = () => {
  //   let startAngle = (this.startRotate * 180) / Math.PI,
  //     awardAngle = (this.awardRotate * 180) / Math.PI,
  //     pointerAngle = 90,
  //     overAngle = (startAngle + pointerAngle) % 360,
  //     restAngle = 360 - overAngle,
  //     index = Math.floor(restAngle / awardAngle)

  //   return items[index];
  // }

  // passed down props
  export let pointerColor = "black";
  export let pointerTextColor = "white";
  export let items = ["yes", "no", "maybe"];
  export let colors = Array.from({ length: items.length }, generateColors);
  export let size = 400;
  export let pointerSize = size / 8;
  export let textColor = "white";

  /* label */
  // $: spinDeg = 360 / items.length;
  // <-- this reactivity is an artifact && was causing large spinDeg differential
  // <-- upon svgRender happening on DOM update

  /* wheel sizes */
  $: radius = Math.min(size, size) / 2;

  const spinWheel = (e) => {
    console.log("SPIN EVENT:", e);
    e.stopPropagation();
    if (!isSpinning) {
      isSpinning = true;
      spinDeg += Math.random() * 8 + 3;

      setTimeout(() => {
        isSpinning = false;
      }, 100);
    }
  };
  let segmentColors = [...colors];

  const colorArrayLengthMatcher = () => {
    if (colors.length !== items.length) {
      segmentColors = Array.from(
        { length: items.length },
        (_, i) => colors[i % colors.length]
      );
    }
  };

  const svgRender = () => {
    select(".wheel svg").remove();
    colorArrayLengthMatcher();
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
      .attr("fill", textColor);
    // .style("rotate", (_, i) => `z ${((360 / items.length) * i )}deg`); /* <-- prob bad */
  };
  /**
   * @description optional approach to render labels from D3 docs
   * @link https://www.d3indepth.com/shapes/#arc-generator
   */
  // svg
  //     .selectAll("mySlices")
  //     .data(dataWithArc)
  //     .join("text")
  //     .each(function (d, i) {
  //       const centroid = arcGenerator.centroid(d);
  //       console.log("centroid[0]", centroid[0]);
  //       console.log("centroid[1]", centroid[1]);
  //       console.log("this", this);
  //       select(this)
  //         .attr("display", "block")
  //         .attr("x", centroid[0])
  //         .attr("y", centroid[1])
  //         .attr("dy", "0.33em,")
  //         .attr("justify-content", "center")
  //         .text(items[i])
  //         .attr("fill", textColor);
  //     });

  afterUpdate(svgRender);
  // beforeUpdate(svgRender)
  // onMount(svgRender);
  // onMount(svgRender);
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
