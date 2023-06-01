<script>
  import Segment from "./Segment.svelte";
  export let items = ['yes', 'no'];
  let deg = 360 / items.length  

</script>


<div class='wheel'>
  {#each items as item, index}
    <Segment {item} {deg} {index}/>
  {/each}
</div>



<style>
  .wheel{
    background: linear-gradient(#e66465, #9198e5);
    height: 100%s;
    aspect-ratio: 1/1;
     clip-path: circle();
      animation: spin 1.3s linear infinite; 
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