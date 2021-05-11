<script>
  import { onMount } from "svelte";
  const socket = new WebSocket("ws://localhost:8080/ws");

  let messages = [];
  let msg = "";

  onMount(() => {
    socket.addEventListener("open", () => {
      console.log("connection opened!");
    });
    socket.addEventListener("message", (e) => {
      messages = [...messages, e.data];
    });
  });

  const sendMessage = () => {
    socket.send(msg);
  };
</script>

<div>
  <input type="text" bind:value={msg} />
  <button on:click={sendMessage}>send</button>
  {#each messages as message}
    <p>{message}</p>
  {/each}
</div>
