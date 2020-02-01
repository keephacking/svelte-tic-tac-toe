<script>
  let gameTurn = "x";
  let squares = [null, null, null, null, null, null, null, null, null];
  let winner = null;

  function restartGame() {
    gameTurn = "x";
    squares = [null, null, null, null, null, null, null, null, null];
    winner = null;
  }
  function handleClick(i) {
    if (!squares[i] && !winner) {
      squares[i] = gameTurn;

      switchTurn();

      winner = calculateWinner(squares);

      if (winner) gameTurn = null;
    }
  }

  function switchTurn() {
    if (gameTurn === "x") gameTurn = "o";
    else gameTurn = "x";
  }

  function calculateWinner(squares) {
    const winningCombo = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ];
    for (let i = 0; i < winningCombo.length; i++) {
      const [a, b, c] = winningCombo[i];
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c])
        return `Winner: ${squares[a].toUpperCase()}`;
    }
    const isDraw = squares.every(square => square !== null);
    return isDraw ? "It's a draw" : null;
  }
</script>

<style type="text/scss" lang="scss">
  :root {
    --cell-size: 100px;
    --mark-size: calc(var(--cell-size) * 0.9);
  }
  .board {
    display: grid;
    justify-content: center;
    justify-items: center;
    align-content: center;
    align-items: center;
    grid-template-columns: repeat(3, auto);
  }
  .cell {
    width: var(--cell-size);
    height: var(--cell-size);
    border: 1px solid black;
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    cursor: pointer;

    &:first-child,
    &:nth-child(2),
    &:nth-child(3) {
      border-top: none;
    }

    &:nth-child(3n + 1) {
      border-left: none;
    }

    &:nth-child(3n + 3) {
      border-right: none;
    }

    &:last-child,
    &:nth-child(8),
    &:nth-child(7) {
      border-bottom: none;
    }
  }

  .cell.x,
  .cell.o {
    cursor: not-allowed;
  }

  .cell.x::before,
  .cell.x::after,
  .cell.o::before {
    background-color: black;
  }

  .board.x .cell:not(.x):not(.o):hover::before,
  .board.x .cell:not(.x):not(.o):hover::after,
  .board.o .cell:not(.x):not(.o):hover::before {
    background-color: lightgrey;
  }

  .cell.x::before,
  .cell.x::after,
  .board.x .cell:not(.x):not(.o):hover::before,
  .board.x .cell:not(.x):not(.o):hover::after {
    content: "";
    position: absolute;
    width: calc(var(--mark-size) * 0.15);
    height: var(--mark-size);
  }

  .cell.x::before,
  .board.x .cell:not(.x):not(.o):hover::before {
    transform: rotate(45deg);
  }

  .cell.x::after,
  .board.x .cell:not(.x):not(.o):hover::after {
    transform: rotate(-45deg);
  }

  .cell.o::before,
  .cell.o::after,
  .board.o .cell:not(.x):not(.o):hover::before,
  .board.o .cell:not(.x):not(.o):hover::after {
    content: "";
    position: absolute;
    border-radius: 50%;
  }

  .cell.o::before,
  .board.o .cell:not(.x):not(.o):hover::before {
    width: var(--mark-size);
    height: var(--mark-size);
  }

  .cell.o::after,
  .board.o .cell:not(.x):not(.o):hover::after {
    width: calc(var(--mark-size) * 0.7);
    height: calc(var(--mark-size) * 0.7);
    background-color: white;
  }
  h1 {
    color: #ff3e00;
    text-transform: capitalize;
    font-size: 4em;
    font-weight: 100;
  }
</style>

{#if winner}
  <h1>{winner}</h1>
  <button on:click={restartGame}>Restart Game</button>
{:else}
  <h1>Next player {gameTurn}</h1>
{/if}
<div class={gameTurn != null ? gameTurn + ' board' : 'board'}>
  {#each squares as square, i}
    <div
      class={square != null ? square + ' cell' : 'cell'}
      on:click={() => handleClick(i)} />
  {/each}
</div>
