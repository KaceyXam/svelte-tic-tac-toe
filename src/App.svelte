<script lang="ts">
  let board: string[] = Array(9).fill("");
  let isXTurn = true;
  let isOver = false;
  let winMessage: string;
  
  $: currentPlayer = isXTurn ? "X" : "O"

  const setTile = (id: number) => {
    if (board[id] !== "") return;
    
    board[id] = currentPlayer
    
    checkWinner();
    isXTurn = !isXTurn;
  }

  const displayWinner = (id: number) => {
    return board[id] === currentPlayer;
  }

  const checkWinner = () => {
    if (board.every((tile) => tile !== "")) {
      isOver = true;
      winMessage = "Draw! Refresh to try again";
      return;
    }

    if (
      board[0] === board[1] && board[1] === board[2] && board[2] !== "" ||
      board[3] === board[4] && board[4] === board[5] && board[5] !== "" ||
      board[6] === board[7] && board[7] === board[8] && board[8] !== "" ||
      board[0] === board[3] && board[3] === board[6] && board[6] !== "" ||
      board[1] === board[4] && board[4] === board[7] && board[7] !== "" ||
      board[2] === board[5] && board[5] === board[8] && board[8] !== "" ||
      board[0] === board[4] && board[4] === board[8] && board[8] !== "" ||
      board[2] === board[4] && board[4] === board[6] && board[6] !== ""
    ) {
      isOver = true;
      winMessage = `${currentPlayer} has won! Refresh to play again!`;
    }
  }
</script>

<main>
  <h1>Svelte TicTacToe</h1>
  <h2>Current Player: {isXTurn ? "X" : "O"}</h2>
  {#if isOver}
  <h2>{winMessage}</h2>
  {/if}
  <section class="board-wrapper">
    {#each board as tile, id}
      <button disabled={isOver} on:click={() => setTile(id)} class="tile">{tile}</button>
    {/each}
  </section>
</main>

<style> 
  main {
    display: flex;
    flex-direction: column;
    gap: 1em;
    align-items: center;
    justify-content: center;
    min-height: 100vh;
  }

  .board-wrapper {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    place-items: center;
    max-width: 20rem;
    width: 100%;
    gap: .25em;
    padding: .25em;
    background-color: black;
  }

  .tile {
    border: none;
    text-align: center;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 3rem;
    width: 100%;
    aspect-ratio: 1 / 1;
    background-color: white;
  }

  .tile:disabled {
    color: black;
  }
</style>