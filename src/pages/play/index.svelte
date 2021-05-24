<script>
  import Typer from "../../components/Typer.svelte";
  import StatsMeter from "../../components/StatsMeter.svelte";
  import { onMount } from "svelte";
  import { INITIALIZATION, START_GAME, PROGRESS_ADVANCE, VICTORY } from '../../constants/messages'
import VictoryModal from "../../components/VictoryModal.svelte";
import CountdownTimer from "../../components/CountdownTimer.svelte";

  const socket = new WebSocket("wss://type-god-server.herokuapp.com/ws");
  let winner = null
  let self
  let text = null;
  let players = [];
  let wordCount = 0
  let wpm = 0
  let minutes
  let seconds
  let finalStats = {
    minutes: 0,
    seconds: 0,
    wpm: 0
  }
  let shouldStartCountdown = false
  let isInputDisabled = true

  const setFinalStats = () => {
      finalStats.minutes = minutes
      finalStats.seconds = seconds
      finalStats.wpm = wpm
  }
  onMount(() => {
    socket.addEventListener("open", () => {
      console.log("connection opened!");
    });
    socket.addEventListener("message", (e) => {
      const parsed = JSON.parse(e.data);
      switch (parsed.event) {
        case INITIALIZATION:
          self = parsed.data.nick
          break;
        case START_GAME:
          text = parsed.data.text;
          players = parsed.data.players.map((p) => ({ progress: 0, nick: p }));
          shouldStartCountdown = true
          break;
        case PROGRESS_ADVANCE:
          const i = players.findIndex((p) => p.nick === parsed.data.player);
          players[i] = { ...players[i], progress: parsed.data.progress };
          break;
        case VICTORY: 
          const j = players.findIndex((p) => p.nick === parsed.data.player);
          players[j] = { ...players[j], progress: 100};
          setFinalStats()
          winner = parsed.data.player
          break;

      }
    });
  });

  const sendMessage = (msg) => {
    socket.send(msg);
  };

 
</script>

{#if text}
<CountdownTimer {shouldStartCountdown} bind:isInputDisabled={isInputDisabled}/>
<VictoryModal {winner} {self} {finalStats}/>
  <div class="container">
    <div class="player-bar-container">
      {#each players as player}
        <div class="player-container">
          <div class="player-bar" style="width: {player.progress}%" class:self-bar="{player.nick === self}" class:winner="{winner === player.nick}">
            <span>{player.progress}%</span>
          </div>
          <small>({player.nick === self ? 'You' : player.nick})</small>
          <br />
        </div>
      {/each}
    </div>
    <div class="typer-container">
      <Typer {text} {sendMessage} bind:wordCount={wordCount} {isInputDisabled}/>
    </div>
    <StatsMeter {wordCount} bind:minutes={minutes} bind:seconds={seconds} bind:wpm={wpm}/>
    
  </div>
{:else}
  <div class="spinner-container">
    <img src="images/spinner.svg" alt="spinner">
    <span>Looking for a room...</span>

  </div>
{/if}

<style>
  .container {
    padding-top: 3rem;
    overflow: hidden;
    width: 100vw;
    height: 100%;
    max-height: 100%;
    background-color: var(--linen);
    display: grid;
    grid-template-columns: 4fr 3fr;
    grid-template-rows: 2fr 2fr;
  }

  .typer-container {
    grid-template-rows: 1/2;
  }
  .player-bar-container {
    padding-left: 1rem;
    display: flex;
    flex-direction: column;
    width: 70%;
    grid-column: 2/3;
    grid-row: 1/2;
  }
  .player-container {
    display: flex;
    flex-direction: column;
    color: var(--gray);
  }
  .player-container small {
    font-size: 0.6em;
    font-weight: bold;
  }
  .player-bar {
    height: 2rem;
    background-color: var(--light-purple);
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0 0.5rem;
  }
  .player-bar span {
    color: white;
    font-weight: bold;
    font-size: 0.9rem;
  }

  .self-bar {
    background-color: var(--purple);
  }

  .winner {
    background: rgb(255,227,5);
background: linear-gradient(270deg, rgba(255,227,5,1) 6%, rgba(215,180,20,1) 100%, rgba(45,34,0,1) 100%);
  }

  .spinner-container {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    font-size: 1.4rem;
    width: 100vw;
    height: 100%;
    background-color: var(--linen);
    color: var(--dark-purple);
  }





</style>
