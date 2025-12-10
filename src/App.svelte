<script>
  //Deals with state of the game, math and UI interaction
  let gameStarted = false;
  let score = 0;
  let time = 0.0; 
  let top = 0;
  let left = 0;
  const boxSize = 50; 
  const BigTBarHeight = 60; 
  let timerInterval; 

  // game logic functions
  function startGame() {
    gameStarted = true;
    resetStats();
    startTimer();
    moveBox();
  }

  function resetStats() {
    score = 0;
    time = 0.0;
  }

  function startTimer() {
    stopTimer();
    timerInterval = setInterval(() => {time = time + 0.1;}, 100);
  }

  function stopTimer() {
    if (timerInterval) clearInterval(timerInterval);
  }

  function handleReset() {
    stopTimer();
    resetStats();
    startTimer();
    moveBox();
  }

  function handleBoxClick() {
    score = score + 1;
    moveBox();
  }

  function moveBox() {
    const maxTop = window.innerHeight - boxSize - BigTBarHeight;
    const maxLeft = window.innerWidth - boxSize;

    // BigTBar + here makes it so it'll never spawn on taskbar
    top = Math.floor(Math.random() * maxTop) + BigTBarHeight;
    left = Math.floor(Math.random() * maxLeft);
  }
</script>

<!--Main block uses boolean if to switch between both pages-->
<main>
  
  {#if !gameStarted}
    <div class="menu">
      <button on:click={startGame}>
        I Want to Work on my Mechanics
      </button>
    </div>

  {:else}
    <nav class="BigTBar">
      <div class="stats-container">
        <div class="stat">Time: <span>{time.toFixed(1)}s</span></div>
        <div class="stat">Score: <span>{score}</span></div>
      </div>
      <button class="resetbutton" on:click={handleReset}>Reset</button>
    </nav>

    <!-- This is the "meat" of the code, the game itself-->
    <div class="game-area">
      <button class="target-box" style="top: {top}px; left: {left}px; width: {boxSize}px; height: {boxSize}px; "on:mousedown={handleBoxClick} >
      </button>
    </div>
  {/if}
</main>

<!--One big eater file, all stylistic choices are below-->
<style>
  
  :global(body) {
    margin: 0;
    padding: 0;
    overflow: hidden;
    display: block !important; 
    width: 100vw;
    height: 100vh;
    background-color: #000000;
  }

  :global(#app) {
    width: 100%;
    height: 100%;
    max-width: none !important; 
    margin: 0 !important;
    padding: 0 !important;
    text-align: left !important; 
  }

  /* Style for first screen */
  main {
    width: 100%;
    height: 100%;
    background-color: rgb(86, 112, 146);
    color: white;
    font-family: Inter, system-ui, 'Times New Roman', Times, serif;
    cursor: crosshair ;
  }


  .menu {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
  }

  .menu button {
    padding: 1.5em 3em;
    font-size: 1.2rem;
    cursor: pointer;
    background-color: #888b57;
    color: white;
    border: none;
    border-radius: 8px;
    font-weight: 600;
    transition: transform 0.1s;
  }

  .menu button:hover {
    background-color: #312f2d;
  }

  .menu button:active {
    transform: scale(0.95);
  }

  /* Style for second screen */
  .game-area {
    position: relative;
    width: 100%;
    height: 100%;
  }

  .target-box {
    position: absolute;
    background-color: #bcc079;
    border: 1px solid #f1f3cf;
    border-radius: 100px;
    cursor: crosshair;
    padding: 0;
    outline: none;
  }

  /* Big TBar is Taskbar at top of game screen */
  .BigTBar {
    height: 60px;
    background-color: #474646; 
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 2rem;
    box-sizing: border-box;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 10;
  }

  .stats-container {
    display: flex;
    gap: 2rem;
  }

  .stat {
    font-size: 1.2rem;
    font-weight: bold;
    font-variant-numeric: tabular-nums;
  }

  .stat span {
    color: #ced0eb; 
    margin-left: 0.5rem;
  }

  .resetbutton {
    padding: 0.5em 1.5em;
    font-size: 1rem;
    cursor: pointer;
    background-color: #888b57;
    color: white;
    border: none;
    border-radius: 8px;
    font-weight: 600;
    transition: transform 0.1s;
  }

  .resetbutton:hover {
    background-color: #312f2d;
  }

  .resetbutton:active {
    transform: scale(0.95);
  }
</style>