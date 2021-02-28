<script>
  // Local varibles
  let displayNewQuiz = false;
  let displayLoadQuiz = false;
  let quizName;

  // global shared variables
  export let currentlyLoadedQuiz;
  export let user;
  export let questionsDB;
  export let quizsDB;

  // Save the inputed quiz into the database
  function saveQuiz() {
    // Get all checked questions ids as a number
    const checkedQuestions = Array.from(document.getElementsByClassName("quiz"))
      .filter((x) => x.getElementsByTagName("input")[0].checked)
      .map((x) => Number(x.firstElementChild.id));
    // Get the checked questions from local database
    const newQuizQuestions = checkedQuestions.map((x) => questionsDB[x]);
    // Make a json object with required elements
    const theJson = {
      name: quizName,
      quizQuestions: newQuizQuestions,
      players: [],
    };
    // Post to databse
    fetch("https://tlk-qoiz-server-prod.herokuapp.com/quizs", {
      method: "POST",
      headers: {
        Authorization: user.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      },
      body: JSON.stringify(theJson),
    })
      .then((data) => data.json())
      .then((result) => (currentlyLoadedQuiz = result.quiz));
  }
  // Load the clicked quiz and then switch to the game view buy clicking the button
  function setLoadedQuiz(i){
    currentlyLoadedQuiz = quizsDB[i.target.id];
    document.getElementById("admin_game").click();
  }
</script>

<div id="addquizes">
  <button on:click={() => (displayNewQuiz = !displayNewQuiz)}>
    <h3>Make a new Quiz</h3>
  </button>
  <button on:click={() => (displayLoadQuiz = !displayLoadQuiz)}>
  <h3>Load old Quiz</h3>
  </button>
  {#if displayNewQuiz}
    <div>
      <p>Name of quiz</p>
      <input type="text" bind:value={quizName} />
      <button on:click={saveQuiz}>Save quiz</button>

      <p>Add questions</p>
      <div id="listofquizes-holder">
        <ul id="listofquizes">
          {#each questionsDB as question, i}
            {#if i<20}
            <li class="quiz">
              {i + 1} <input type="checkbox" name="" id={i} />
              <span> {question.question} </span>
            </li>
            {/if}
          {/each}
        </ul>
      </div>
      <br />
    </div>
  {/if}
  {#if displayLoadQuiz}
    <div>
      <ul>
      {#each quizsDB as quiz, i}
        <li>
          <button id="{i}" on:click={setLoadedQuiz}>
            {quiz.name}
          </button>
            <ul>
            {#each quiz.quizQuestions as question}
            <li>{question.question}</li>
            {/each}
          </ul>
        </li>
        {/each}
      </ul>
    </div>
  {/if}
</div>

<style>
  ul {
    list-style-type: none;
}
</style>