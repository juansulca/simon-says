<script>
  let gameStarted = $state(false);
  let animationPlaying = $state(false);
  let sequence = $state([1, 2]);
  let blinkGreen = $state(false);
  let blinkRed = $state(false);
  let blinkYellow = $state(false);
  let blinkBlue = $state(false);
  let input = $state([]);
  let index = $state(0);

  function onClickGreen() {
    if (animationPlaying) return;
    handleIntpu(1);
  }

  function onClickRed() {
    if (animationPlaying) return;
    handleIntpu(2);
  }

  function onClickYellow() {
    if (animationPlaying) return;
    if (!gameStarted) {
      startGame();
      return;
    }
    handleIntpu(3);
  }

  function onClickBlue() {
    if (animationPlaying) return;
    handleIntpu(4);
  }

  function getRandomValue() {
    const min = 1;
    const max = 4;
    return Math.floor(Math.random() * (max - min + 1) + min);
  }

  function generateSequence() {
    sequence.push(getRandomValue());
  }

  function restart() {
    input = [];
    index = 0;
    sequence = [];
    gameStarted = false;
    resetAllColors();
  }

  function resetAllColors() {
    blinkGreen = false;
    blinkRed = false;
    blinkYellow = false;
    blinkBlue = false;
  }

  function playSequence() {
    const seq = [...sequence];
    animationPlaying = true;
    const interval = setInterval(() => {
      // console.log('seq====>', seq);
      resetAllColors();
      if (seq.length == 0) {
        clearInterval(interval);
        animationPlaying = false;
        return;
      }

      switch (seq.shift()) {
        case 1:
          blinkGreen = true;
          break;
        case 2:
          blinkRed = true;
          break;
        case 3:
          blinkYellow = true;
          break;
        case 4:
          blinkBlue = true;
          break;
      }
    }, 1200);
  }

  function startGame() {
    gameStarted = true;
    generateSequence();
    console.log(...sequence);
    playSequence();
  }

  function handleIntpu(entry) {
    input.push(entry);
    if (input.length < sequence.length) {
      if (checkSequence()) {
        gameOver();
        return;
      }
      index++;
      return;
    }
    if (input.length === sequence.length) {
      if (checkSequence()) {
        gameOver();
        return;
      }
      input = [];
      index = 0;
      generateSequence();
      playSequence();
    }
  }

  function checkSequence() {
    return input[index] !== sequence[index];
  }

  function gameOver() {
    blinkGreen = true;
    blinkRed = true;
    blinkYellow = true;
    blinkBlue = true;
    console.log("you lost!");
    setTimeout(() => {
      restart();
    }, 2000);
  }
</script>

<h1>Simon</h1>
<p>press yellow to start</p>
<div>game: {gameStarted}</div>
<button onclick={restart}>restart</button>
<div class="container">
  <div class="square green" class:blink={blinkGreen} onclick={onClickGreen}>
    1
  </div>
  <div class="square red" class:blink={blinkRed} onclick={onClickRed}>2</div>
  <div class="square yellow" class:blink={blinkYellow} onclick={onClickYellow}>
    3
  </div>
  <div class="square blue" class:blink={blinkBlue} onclick={onClickBlue}>4</div>
</div>

<style>
  .container {
    display: flex;
  }

  .square {
    width: 5rem;
    height: 5rem;
    color: #000000;
    border: 1px solid #000000;
  }

  .green {
    background: #51ff00;
  }
  .red {
    background: #e60000;
  }
  .yellow {
    background: #ffff3a;
  }
  .blue {
    background: #0163c6;
  }

  @keyframes blink {
    0%,
    100% {
      opacity: 1;
    }
    50% {
      opacity: 0;
    }
  }

  .blink {
    animation: blink 1.2s linear infinite;
  }
</style>
