<script>
  import Board from "./lib/Board.svelte";

  let result = null;

  $: result = result;
</script>

<main class:error={result === 0}
      class:success={result === 1}>
  <Board bind:result={result}/>
</main>

<style>
</style>
