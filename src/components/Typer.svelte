<script>
  let text = `As she sat watching the world go by, something caught her eye. It wasn't so much its color or shape, but the way it was moving. She squinted to see if she could better understand what it was and where it was going, but it didn't help. As she continued to stare into the distance, she didn't understand why this uneasiness was building inside her body. She felt like she should get up and run. If only she could make out what it was. At that moment, she comprehended what it was and where it was heading, and she knew her life would never be the same.`;
  let inputText = "";
  let doneText = "";
  let error = false;

  const evaluateText = (input, text) => {
    length = input.length;
    if (input !== text.substr(0, length)) {
      error = true;
      return;
    }
    doneText = input;
    error = false;
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

  <input type="text" bind:value={inputText} />

  <div class="result" class:error />
</div>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  .text-container {
    border: 1px solid black;
    padding: 1rem;
    overflow-wrap: break-word;
  }

  input {
    padding: 0.3rem;
    margin: 3rem auto;
    width: 80%;
  }

  .result {
    width: 3rem;
    height: 3rem;
    background-color: rgb(83, 255, 98);
  }
  .next-letter {
    background-color: rgb(233, 190, 190);
  }

  .error {
    background-color: red;
  }

  .done-text {
    color: red;
  }
</style>
