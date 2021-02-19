<script>
  export let waitingRoom = [];
  export let socket;
  export let currentlyLoadedQuiz;
  let gameStats = [];

  $: playerScore = gameStats.map((player, i) => {
    let t = {
      index: i,
      playerID: player.playerID,
      socketID: player.socketID,
      player: player.player,
      questions: player.questions.map((q, i) => q.question),
      correctAnswers: player.questions.map((q, i) => q.correctAnswers),
      playerAnswers: player.questions.map((q, i) => player.answers[i]),
      trueOrFalse: player.questions.map((q, i) =>
        q.correctAnswers
          .map((x) => x.toLowerCase())
          .includes(player.answers[i].toLowerCase())
      ),
      score: player.questions
        .map((q, i) =>
          q.correctAnswers
            .map((x) => x.toLowerCase())
            .includes(player.answers[i].toLowerCase())
        )
        .filter((x) => x == true).length,
    };
    return t;
  });

  socket.on("adminGameOver", (data) => {
    console.log(data);
    gameStats = [...gameStats, data];
    console.log(playerScore);
  });

  function startGame() {
    socket.emit("admin", { type: "startGame", quiz: currentlyLoadedQuiz });
  }

  function changeTrueFalse(t) {
    const target = t.target.id.split(" ");
    const playerIndex = playerScore.filter((x) => x.player == target[0]);
    playerScore[playerIndex[0].index].trueOrFalse[target[1]] = !playerScore[
      playerIndex[0].index
    ].trueOrFalse[target[1]];
    playerScore[playerIndex[0].index].score = playerScore[
      playerIndex[0].index
    ].trueOrFalse.filter((x) => x == true).length;
  }

  function sendResults() {
    let data = {
      game: gameStats[0].game,
      playerScore: playerScore,
      type: "closeGame",
    };
    socket.emit("admin", data);
  }
</script>

<div>
  {#if currentlyLoadedQuiz}
    <h4>Game Info:</h4>
    <p>Currently Loaded Quiz: {currentlyLoadedQuiz?.name}</p>
    <button on:click={startGame}>Start Game</button>
  {/if}
  <h4>Players in Waiting Room:</h4>
  <ul>
    {#each waitingRoom as player}
      <li>{player.name}</li>
    {/each}
  </ul>

  {#if gameStats.length > 0}
    <ul>
      {#each playerScore as player}
        <div id={player.player} class="player_bar">
          <h3>
            {player.player} Score: {player.score}/{player.playerAnswers.length}
          </h3>
          {#each player.questions as question, i}
            <li
              class={player.trueOrFalse[i] ? "green" : "red"}
              id={"li" + player.player + i}
            >
              <h4>Question: {question}</h4>
              <p class="result_answers">
                <span>Correct Answers: {player.correctAnswers[i]} </span>
                <span>Player Answers: {player.playerAnswers[i]} </span>
                <input
                  type="checkbox"
                  id={player.player + " " + i}
                  on:click={changeTrueFalse}
                  checked={player.trueOrFalse[i] ? true : false}
                  name=""
                />
              </p>
            </li>
          {/each}
        </div>
      {/each}
    </ul>
    <button on:click={sendResults}>Send Results</button>
  {/if}
</div>

<style>
  ul {
    list-style-type: none;
    padding: 0;
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
  .player_bar li{
    margin: 1em 10em;
    border-radius: 10em;
    padding: 0.5rem;
  }
</style>
