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
    top: 0;
    margin: 10px auto ;
    background-color: aqua;
    width: fit-content;
    height: 5%;
    border-radius: 0 0 100% 100% ;
  }
</style>