<script>
  import Board from "./Board.svelte";
</script>

<style>
  main {
    text-align: center;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>

<main>
  <Board />
</main>
