<script>

////// VARIABLES 

  import {fly,fade} from "svelte/transition";
  import WaitingRoom from "./WaitingRoom.svelte";
  import _ from 'lodash';

  export let user;
  export let socket;
  let readyState = false;
  let message;
  let currentGame;
  let currentQuestion;
  let timer;
  let timerClock;
  let playerAnswer;
  let correctAnswers;
  let gameScore;
  let lockAnswer = false;
  let gameOver = false;

///// FUNCTIONS

  function getReady() {
    console.log("sending");
    const data = { name: user.id[0].username, id: user.id[0]._id };
    readyState
      ? socket.emit("removeplayer", data)
      : socket.emit("newplayer", data);
    readyState = !readyState;
  }

  function startTimer() {
    console.log("starting timer");
    timer = setInterval(() => {
      if (timerClock <= 0) {
        stopTimer();
      } else {
        timerClock = timerClock - 1;
      }
    }, 1000);
  }
  function stopTimer() {
    clearInterval(timer);
    console.log("stop timer");
    socket.emit("roundFinished", {
      playerID: user.id[0]._id,
      player: user.id[0].username,
      answer: playerAnswer,
      game: currentGame,
    });
    let el = document.getElementById("answer_input");
    if (el?.disabled) {
      lockAnswerInput();
    }
  }
  function lockAnswerInput() {
    let el = document.getElementById("answer_input");
    el.disabled = !el.disabled;
    lockAnswer = el.disabled;
  }
  function pressEnter(ev) {
    if (ev.keyCode == 13) {
      let btn = document.getElementById("submit_button");
      btn.click();
    }
  }
  function init(el) {
    el.focus();
  }

///// SOCKETS 
  socket.on("messageChannel", (data) => {
    console.log(data);
    message = data;
  });

  socket.on("starting", (data) => {
    currentGame = data.gameID;
    timerClock = data.time;
    startTimer();
  });
  socket.on("gameOver", (data) => {
    message = { message: "Game Over Thanks for Playing!", list: [] };
    currentQuestion = "";
    gameOver = true;
    // correctAnswers = data;
  });
  socket.on("newRound", (data) => {
    playerAnswer = "";
    timerClock = data.time;
    currentQuestion = data.question;
    startTimer();
  });
  socket.on("gameResults", (data) => {
    gameScore = data.score;
    correctAnswer = _.map(data.questions,(x, i) => [
      x,
      data.correctAnswers[i],
      data.playerAnswers[i],
      data.trueOrFalse[i]]);
   // correctAnswers = data.questions.map((x, i) => [
   //   x,
   //   data.correctAnswers[i],
   //   data.playerAnswers[i],
   //   data.trueOrFalse[i],
   // ]);
  });
</script>

<WaitingRoom {socket}></WaitingRoom>

<div>
  <h3>Hello {user.id[0].username}</h3>
 
  {#if message}
    <p transition:fade>
      {message.message}
    </p>
  {/if}
  {#if currentQuestion}
    <p transition:fly="{{ y: 200, duration: 2000 }}">{currentQuestion}</p>
    <div>
      <input
        id="answer_input"
        type="text"
        on:keyup={pressEnter}
        bind:value={playerAnswer}
        use:init
      />
      <button id="submit_button" on:click={lockAnswerInput}
        >{!lockAnswer ? "Submit" : "Abort!"}</button
      >
    </div>
  {/if}
  {#if !currentGame}
    <button on:click={getReady}
      >{readyState ? "Abort Quizzing" : "I am ready for Quizzing!"}</button
    >
  {/if}
  {#if gameOver && !correctAnswers}
    <h3 transition:fade>Please wait while the GameMaster checks your answers!</h3>
    <p>Don't Leave This Page!!!</p>
  {/if}
  {#if correctAnswers}
    <h2>Results:</h2>
    <h3>Your Score: {gameScore}</h3>
    <ul>
      {#each correctAnswers as question, i}
        <li class={question[3] ? "player_bar green" : "player_bar red"}>
          <h4>{question[0]}</h4>

          <p class="result_answers">
            <span>Correct Answer: {question[1]} </span><span>
              Your Answer: {question[2]}
            </span>
          </p>
        </li>
      {/each}
    </ul>
  {/if}
  {#if timerClock}
    <h2>
      {timerClock}
    </h2>
  {/if}

</div>

<style>
  ul {
    padding: 0em;
    list-style-type: none;
  }
  .result_answers {
    display: flex;
    justify-content: space-around;
  }
  .red {
    background-color: rgba(255, 0, 0, 0.3);
  }
  .green {
    background-color: rgba(0, 255, 0, 0.3);
  }
  .player_bar{
    margin: 1em 10em;
    border-radius: 10em;
    padding: 0.5rem;
  }
</style>
