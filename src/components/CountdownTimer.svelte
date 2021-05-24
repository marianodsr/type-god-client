<script>
  import { onDestroy } from "svelte";
  export let shouldStartCountdown = true;
  let counter = 5;
  const interval = setInterval(() => {
    counter--;
    if (counter === 0) shouldStartCountdown = false;
  }, 1000);

  onDestroy(() => {
    clearInterval(interval);
  });
</script>

{#if shouldStartCountdown}

  
  <div class="container">
    <h2>{counter}</h2>
  </div>

{/if}

<style>

  .container {
    border-radius: 50%;
    border: 1rem solid var(--purple);
    width: 15rem;
    height: 15rem;
    background-color: var(--linen);
    color: var(--purple);
    font-size: 5rem;
    font-weight: bold;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  h2 {
      margin: 0;
      padding: 0;
  }
</style>
