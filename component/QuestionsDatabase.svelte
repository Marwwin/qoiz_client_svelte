<script>
  import { onMount } from "svelte";
  import _ from "lodash";
  import {fade, fly} from 'svelte/transition';

  // Local variables
  let pageNumber = 0;
  let prevNumber = 0;
  let direction = true;

  // Computed variables
  // This tries to control the animation when scrolling thru questions
  // It works but a bit funky when changing direction 
  $: { 
    if (pageNumber <= prevNumber){
      direction = false;
    }
    else {
      direction = true;
    }
    prevNumber = pageNumber;
  }

  let questionsDB;


// This is not in use anymore
// Can Probably Be Removed
  function populateQuestions() {
    fetch("https://tlk-qoiz-server-prod.herokuapp.com/questions/")
      .then((res) => res.json())
      .then((data) => {
        questionsDB = _.chunk(data, 20);
      });
  }
  onMount(async () => {
    populateQuestions();
  });

</script>

<span>
  <button on:click={() => pageNumber--} disabled={pageNumber == 0 ? true : false} > Prev </button>
  <button on:click={() => pageNumber++} disabled={pageNumber == questionsDB.length-1}> Next </button>
</span>
{#if questionsDB}
  {#each questionsDB as question, i}
    {#if pageNumber == i}
    <div class="outer">
    <ul in:fly={direction ? { x: 1000, duration: 1000 } : { x: -1000, duration: 1000 }} out:fly={direction ? { x: -1000, duration: 1000 } : { x: 1000, duration: 1000 }} class="page">
      <p  >Page {i+1}</p>
      {#each question as q}
        <p class="question">{q.question}</p>
      {/each}
    </ul>
  </div>
    {/if}
  {/each}
  
{/if}

<style>
  ul {
    padding: 0em;
    list-style-type: none;
  }
  .show{
   display:block;
  }
  .hidden{
    display: none;
  }
  .outer{
    position: absolute;
    left:50%
  }
  .page{
    position: relative;
    left:-50%;
  }
  .question{
    margin:0.2rem;
    padding:0.2rem;
  }

</style>
