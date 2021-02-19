<script>
  import { Link } from "svelte-routing";
  export let user;
  import _ from 'lodash';
  

</script>

<div id="home">
  <h1>QUIZ APP F*CK YEAH!!!!</h1>
  <h2>Start to play!</h2>
  <p>And enjoy fun quiz games with your friends!</p>
  {#if user?.id}
    <Link to="game"> <button>Start Quizzing!</button> </Link>
  {:else}
    <Link to="login"><button>Log In to start Quizzing!</button></Link>
  {/if}
  {#if user?.type == "admin"}
  <Link to="admin"><button>Open Admin Panel</button> </Link>
  {/if}
</div>

<style>


</style>
