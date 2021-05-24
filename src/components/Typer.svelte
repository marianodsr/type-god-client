<script>
  export let text = "";
  export let sendMessage;
  export let wordCount;
  export let isInputDisabled
  let inputText = "";
  let doneText = "";
  let error = false;
  let offset = 0;


  // The bear roars
  // OFFSET  0  0 0  0
  // INPUT   '' T Th The
  // DT      '' T Th The


  const evaluateText = (input, text) => {
    if (input.length <= 0) return;
    length = input.length;
    if (input !== text.substr(offset, length)) {
      error = true;
      return;
    }
    const doneWords = doneText.split(" ");
    const lastWord = doneWords[doneWords.length - 1];
    if (length <= lastWord.length) return;

    doneText = doneText.concat(input[input.length - 1]);
    error = false;
 
    if (input[input.length - 1] === " ") {
      //Word break
      offset += input.length;
      inputText = "";
      wordCount++;
      console.log(wordCount);
    }

    sendMessage(
      JSON.stringify({
        event: "PROGRESS_ADVANCE",
        data: {
          doneText: doneText,
        },
      })
    );
  };

  const checkForWin = (doneText, text) => {
    if (doneText === text) return;
  };

  $: evaluateText(inputText, text);
  $: checkForWin(doneText, text);
</script>

<div class="container">
  <div class="text-container">
    <p>
      <span class="done-text">{doneText}</span><span class="next-letter"
        >{#if doneText.length !== text.length}{text[doneText.length]}{/if}</span
      >{text.substr(doneText.length + 1, text.length)}
    </p>
  </div>

  <input type="text" bind:value={inputText} disabled={isInputDisabled} />
</div>



<style>
  .container {
    display: flex;
    max-height: 100%;
    flex-direction: column;
    align-items: center;
  }
  .text-container {
    border: 1px solid var(--gray);
    padding: 1rem;
    overflow-wrap: break-word;
    width: 90%;
    color: var(--gray);
    font-size: 1.2rem;
    -webkit-user-select: none;
    -webkit-touch-callout: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }

  input {
    padding: 0.3rem;
    margin: 3rem auto;
    width: 90%;
    font-size: 1.2rem;
    background-color: var(--linen);
    border: var(--gray) 1px solid;
    color: var(--gray);
  }
  input:focus {
    outline: none;
  }

  .result {
    width: 3rem;
    height: 3rem;
    background-color: rgb(83, 255, 98);
  }
  .next-letter {
    background-color: var(--light-purple);
    color: white;
  }

  .error {
    background-color: red;
  }

  .done-text {
    color: var(--light-purple);
  }
</style>
