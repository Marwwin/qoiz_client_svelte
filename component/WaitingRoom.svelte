<script>

import {onMount, tick} from 'svelte'
import {fly,fade} from "svelte/transition"

export let socket;
let waitingRoom;

function getWR() {
    socket.emit("update-waiting-room");

  }
  socket.on("waiting-room", (data) => {
    waitingRoom = data;
  });

  onMount(async () => {
    getWR();
    await tick()
    let el = document.getElementById("waiting_room");
    el.style.left = window.innerWidth - el.clientWidth +"px";
    el.style.top = document.getElementById("head").clientHeight + "px";
  });

</script>

<div id="waiting_room">
  {#if waitingRoom}
  <h5>Players in Room</h5>
  <ul style="left: {window.innerWidth}">
   {#each waitingRoom.list as player}
   <li in:fly="{{ y: 200, duration: 1000 }}" out:fade>
      {player.name}
   </li>
   {/each}
 </ul>
 {/if}

</div>

<style>
  #waiting_room{
    position: absolute;
    width: 15em;
    left: 0;
    top: 0;
  }
  ul{
    padding: 0em;
    list-style-type: none;
  }
  li{
    font-size: small;
  }
</style>