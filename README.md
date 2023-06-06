<h1 align="center"><b>Svelte-Custom-Roulette</b></h1>

<p align="center">
  <a href="./UNLICENSE" target="blank"><img src="https://img.shields.io/badge/license-UNLICENSE-blue?style=plastic"></img></a>
  <a href="https://github.com/jor-col/svelte-custom-roulette" target="blank"><img src="https://img.shields.io/github/commit-activity/w/jor-col/svelte-custom-roulette?style=plastic"></img></a>
  <a href="https://github.com/jor-col/svelte-custom-roulette" target="blank"><img src="https://img.shields.io/github/contributors/jor-col/svelte-custom-roulette?style=plastic"></img></a>
  <a href="https://github.com/jor-col/svelte-custom-roulette" target="blank"><img src="https://img.shields.io/github/languages/count/jor-col/svelte-custom-roulette?style=plastic&color=orange"></img></a>
  <a href="https://github.com/jor-col/svelte-custom-roulette" target="blank"><img src="https://img.shields.io/github/search/jor-col/svelte-custom-roulette/goto?style=plastic"></img></a>
  <a href="https://github.com/jor-col/svelte-custom-roulette" target="blank"><img src="https://img.shields.io/github/languages/code-size/jor-col/svelte-custom-roulette?style=plastic"></img></a>
  <a href="https://github.com/jor-col/svelte-custom-roulette" target="blank"><img src="https://img.shields.io/github/directory-file-count/jor-col/svelte-custom-roulette?color=green&style=plastic"></img></a>
</p>
<p align="center">
  <a href="https://github.com/jor-col/svelte-custom-roulette" target="blank"><img src="https://img.shields.io/github/forks/jor-col/svelte-custom-roulette?style=social"></img></a>
</p>
<br />
<div align="center">
  <h2><b><a href="#experimental" aria-hidden="true" color="white">W.I.P</a></b></h2>
  <i>As of 6/05/23 this Library is pre-release (v0.1.1). It is stable and can accept props, however, custom labels need to be rotated.</i> <a href="https://www.npmjs.com/package/svelte-custom-roulette" target="blank">published npm package</a>
</div>

<hr />
<br />

<div align="center">
  <h2><b>Installation</b></h2>
</div>

```javascript
$ npm i svelte-custom-roulette
```

<br />
<h2 align="center"><b>
  Quick Start
</b></h2>

```javascript
<script>
  import { Wheel } from "svelte-custom-roulette";

  const items = [
    "item 1",
    "item 2",
    "item 3",
  ]

  <Wheel
    {items}
    size={800}
    textColor="black"
    pointerColor="purple"
    pointerSize={60}
    >

</script>
```

<div class="table-container" align="center">
<h2><b>API</b></h2>
<article itemprop="text">
  <table>
    <thead>
      <tr>
        <th>Props</th>
        <th>Type</th>
        <th>Default</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td><code>items</code></td>
        <td>String[]</td>
        <td><code>["yes", "no", "maybe"]</code></td>
        <td>pass an array to populate wheel segments with custom items to documentation inside the README</td>
      </tr>
      <tr>
        <td><code>size</code></td>
        <td>Number</td>
        <td><code>400</code></td>
        <td>pass a number for the size of the wheel, aspect ratio is 1</td>
      </tr>
      <tr>
        <td><code>colors</code></td>
        <td>String[]</td>
        <td><code>[randomly generated === items.length]</code></td>
        <td>pass an array of strings representing the colors you would like to override wheel segments. <i>this prop is <a href="#experimental">experimental</a></i></td>
      </tr>
      <tr>
        <td><code>textColor</code></td>
        <td>String</td>
        <td><code>"white"</code></td>
        <td>pass a color to override the color of wheel segments' text to documentation inside the README</td>
      </tr>
      <tr>
        <td><code>pointerColor</code></td>
        <td>String</td>
        <td><code>"black"</code></td>
        <td>pass a string for the color of the pointer you want</td>
      </tr>
      <tr>
        <td><code>pointerSize</code></td>
        <td>Number</td>
        <td><code>size / 8</code></td>
        <td>pass a number to size the pointer in <code>px</code> to the center of the wheel, by default this will be 1/8 of the wheel size</td>
      </tr>
    </tbody>

  </table>
</article>
</div>
<br />
<br />
<hr />
<br />
<h2 align="center"><b>Contribute</b></h2>
<b>The JoCo team warmly welcomes contributions to all of our <em>Open Source</em> projects.</b>
<b>Svelte-Custom-Roulette's contribution protocol is as follows:</b>
<br />
<ul>
<br />
<li>Star and Fork the repository</li>
<li>Create a feature branch</li>
<li>We are fans of <a href="https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines">Angular Git Commit Protocol</a> so please, loosely, follow this structure; including header & body syntax including scope and files changed inside your commit message</li>
<li>Submit your Pull Request</li>
</ul>

<br />
<h2 align="center"><b>License</b></h2>
Svelte Custom Roulette is <a href="http://unlicense.org/">UNLICENSE</a> licensed. Please propogate within whatever circumstances you wish.

<!-- [UNLICENSE](/UNLICENSE) -->

<!-- license -->

<!-- [![License](https://img.shields.io/badge/license-UNLICENSE-blue?style=plastic)](./UNLICENSE) -->

<!-- commits -->

<!-- [![License](https://img.shields.io/github/commit-activity/w/jor-col/svelte-custom-roulette?style=plastic)](./README) -->

<!-- contributors -->

<!-- [![License](https://img.shields.io/github/contributors/jor-col/svelte-custom-roulette?style=plastic)](./README) -->

<!-- languages -->

<!-- [![License](https://img.shields.io/github/languages/count/jor-col/svelte-custom-roulette?style=plastic&color=orange)](./README) -->

<!-- goto visits -->

<!-- [![License](https://img.shields.io/github/search/jor-col/svelte-custom-roulette/goto?style=plastic)](./README) -->

<!-- size -->

<!-- [![License](https://img.shields.io/github/languages/code-size/jor-col/svelte-custom-roulette?style=plastic)](./README) -->

<!-- files -->

<!-- [![License](https://img.shields.io/github/directory-file-count/jor-col/svelte-custom-roulette?color=green&style=plastic)](./README) -->

<!-- forks -->

<!-- [![License](https://img.shields.io/github/forks/jor-col/svelte-custom-roulette?style=social)](./README) -->
