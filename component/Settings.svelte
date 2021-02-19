<script>
  import { onMount } from "svelte";

  export let socket;

  onMount(async () => {
    let el = document.getElementById("settings");
  });
</script>

<div id="settings">
  Profile Settings
  <p>{socket.connected ? "Connected" : "Disconnected"}</p>
</div>

<style>
  #settings {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    position: fixed;
    padding: 0.5em;
    margin:0.5em;
    font-size: smaller;
    border:1px solid black;
    border-radius: 15px;
  }
</style>
