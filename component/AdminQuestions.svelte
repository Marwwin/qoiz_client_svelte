<script>

  import QuestionsDatabase from "./QuestionsDatabase.svelte"

  let answers = [];
  let newQuestion = "";
  let newAnswer = "";
  let answerTime = 15;
  let answerValue = 10;
  
  export let questionsDB = [];
  export let user;

  // Add the current answer to the list of answers
  function addAnswer() {
    answers = [...answers, newAnswer];
  }
  // removes the selected answer from the list
  function removeAnswer(i) {
    answers = answers
      .slice(0, i.target.value)
      .concat(answers.slice(i.target.value + 1));
  }
  // Save the question and send it to the server
  function saveQuestion() {
    console.log(user.token)
    const theJson = {
      question: newQuestion,
      correctAnswers: newAnswer,
      questionValue: answerValue,
      answerTime: answerTime,
    };
    fetch("https://tlk-qoiz-server-prod.herokuapp.com/questions/", {
      method: "POST",
      headers: {
        Authorization: user.token,
        Accept: "application/json",
        "Content-Type": "application/json",
      },
      body: JSON.stringify(theJson),
    }).then((res) => {
      console.log("Server Response:")
      console.log(res);
    });
  }
</script>

<div>
  <h3>Add new question</h3>
  <div>
    <p>Question</p>

    <textarea
      rows="5"
      cols="80"
      type="text"
      bind:value={newQuestion}
      name="question"
      id="question"
    />
    <p>Answers</p>
    <input type="text" bind:value={newAnswer} name="answer" id="answer" />
    <button on:click={addAnswer}>+</button>
    <ul>
      {#each answers as answer, i}
        <li>
          <span> {answer} </span>
          <button value={i} on:click={removeAnswer}> - </button>
        </li>
      {/each}
    </ul>
    <p>Time (default 15)</p>
    <input type="number" bind:value={answerTime} name="time" id="time" />
    <p>Value points</p>
    <input type="number" bind:value={answerValue} name="value" id="value" />
    <br />
    <input type="button" value="SAVE QUESTION" on:click={saveQuestion} />
  </div>
</div>
<!--
<div>
  <h2>Questions in database</h2>
  <ul>
    {#each questionsDB as q}
      <li>
        <span>{q.question}</span>
      </li>
    {/each}
  </ul>
</div>
-->
<QuestionsDatabase></QuestionsDatabase>
<style>
  ul {
    list-style-type: none;
}
</style>
