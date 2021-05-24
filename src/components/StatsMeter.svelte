<script>
  import { onDestroy } from 'svelte'
    export let wordCount = 0
    export let minutes = 0
    export let seconds = 0
    export let wpm = 0
    let c = 0

    let startTime = Date.now()
    let elapsedTime = 0

    onDestroy(() => {
      clearInterval(timer)
    })
    const timer = setInterval(( ) => {
          c++
          if(c === 4)
            startTime = Date.now()
          if(c > 5) {
            elapsedTime = Date.now() - startTime
            seconds = Math.floor(elapsedTime / 1000)
            minutes = Math.trunc(elapsedTime / 1000 / 60)
            seconds = seconds - (60 * minutes)
          }

         
    }, 1000)

  

    const calculateWPM = ( wordCount, currentMs ) => {
        const minutes = currentMs / 1000 / 60
        if (minutes === 0) return
        wpm = Math.trunc(wordCount / minutes)
    }

    $: calculateWPM(wordCount, elapsedTime)
</script>


<div class="statistics">
    <span class="time-elapsed">
        {minutes < 10 ? '0' : ''}{minutes}:{seconds < 10 ? '0' : ''}{seconds}
    </span>
    <span>
        {wpm} WPM
    </span>
  </div>





<style>
    
  .statistics {
    grid-row: 2/3;
    grid-column: 2/3;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    font-size: 1.7rem;
    font-weight: bold;
    color: var(--dark-purple);
    max-height: 100%;
    height: 2rem;
  }

  .time-elapsed {
    font-size: 1.7rem;
    margin-right: 3rem;
  }
</style>