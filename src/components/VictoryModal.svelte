<script>
  export let winner = null;
  export let self = null;
  export let finalStats

</script>

{#if winner}
  <div class="background">
    <div class="container">
      <h2 class="title">{winner === self ? "Victory!" : "Defeat"}</h2>
      <div class="stats-container">
        <span>
          WPM {finalStats.wpm}
        </span>
        <span>
            {finalStats.minutes < 10 ? '0' : ''}{finalStats.minutes}:{finalStats.seconds < 10 ? '0' : ''}{finalStats.seconds}
        </span>
      </div>
      <button on:click={() => location.reload()}> PLAY AGAIN </button>
      <span class="winner">Winner: {winner === self ? 'You!' : winner}</span>
    </div>
  </div>
{/if}

<style>
  .background {
    width: 100vw;
    height: 100vh;
    background-color: rgba(0, 0, 0, 0.65);
  }

  .container {
    background-color: white;
    width: 50vw;
    height: 60vh;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    color: var(--gray);
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-evenly;
    border-radius: 4px;
    z-index: 999;
  }
  .title {
    font-size: 3.5rem;
    font-weight: bold;
    text-align: center;
    margin: 0;
    padding: 0;
  }
  .stats-container {
    display: flex;
    width: 50%;
    margin-right: 2rem;
    justify-content: space-between;
    font-weight: bold;
    font-size: 1.5rem;
  }

  button {
    background-color: var(--purple);
    color: white;
    font-weight: bold;
    font-size: 2rem;
    border-radius: 8px;
    cursor: pointer;
  }

  button:hover {
      background-color: var(--light-purple);
      transform: scale(0.9);
  }

  .winner {
    font-weight: bold;
    font-size: 1.2rem;
    max-width: 90%;
    text-align: center;
  }
</style>
