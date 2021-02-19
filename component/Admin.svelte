<script>
  import { Router, Route, Link } from "svelte-routing";
  import { onMount } from "svelte";
  import AdminQuestions from "./AdminQuestions.svelte";
  import AdminQuiz from "./AdminQuiz.svelte";
  import AdminGame from "./AdminGame.svelte";

  export let socket;
  let currentlyLoadedQuiz;
  export let user;
  let waitingRoom;

  let displayQuestionsPanel = false;
  let displayGamePanel = false;
  let displayQuizPanel = false;

  function showAdminElement(ev) {
    if (ev.target.innerHTML == "Game") {
      displayGamePanel = !displayGamePanel;
      displayQuizPanel = false;
      displayQuestionsPanel = false;
    } else if (ev.target.innerHTML == "Quiz") {
      displayQuizPanel = !displayQuizPanel;
      displayGamePanel = false;
      displayQuestionsPanel = false;
    } else if (ev.target.innerHTML == "Questions") {
      displayQuestionsPanel = !displayQuestionsPanel;
      displayGamePanel = false;
      displayQuizPanel = false;
    }
  }

  let questionsDB;
  let quizsDB;

  function populateQuestions() {
    fetch("https://tlk-qoiz-server-prod.herokuapp.com/questions/")
      .then((res) => res.json())
      .then((data) => {
        questionsDB = data;
      });
  }
  function populateQuizs() {
    fetch("https://tlk-qoiz-server-prod.herokuapp.com/quizs/")
      .then((res) => res.json())
      .then((data) => {
        quizsDB = data;
      });
  }
  function getWR() {
    console.log("get Waiting room");
    socket.emit("admin", { type: "getWaitingRoom" });
  }
  socket.on("admin-waiting-room", (data) => {
    console.log(data);
    waitingRoom = data;
  });

  onMount(async () => {
    populateQuestions();
    populateQuizs();
    getWR();
  });
</script>

<h1>This is an admin panel</h1>
<button on:click={showAdminElement}>Questions</button>
<button on:click={showAdminElement}>Quiz</button>
<button id="admin_game"
  on:click={showAdminElement}
  on:click={() => (displayGamePanel ? getWR() : false)}>Game</button
>

{#if displayQuestionsPanel}
  <AdminQuestions {questionsDB} {user} />
{/if}
{#if displayGamePanel}
  <AdminGame {waitingRoom} {currentlyLoadedQuiz} {socket} />
{/if}
{#if displayQuizPanel}
  <AdminQuiz bind:currentlyLoadedQuiz {quizsDB} {user} {questionsDB} />
{/if}


