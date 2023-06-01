<script>
  export let item
  export let deg
  export let index
</script>


  <p 
    style='
    transform: rotate({deg * index}deg)
    '>{item}</p>

<style>
  p{
    margin: 10px auto ;
    background-color: aqua;
    width: fit-content;
    height: 50%;
  }
</style>