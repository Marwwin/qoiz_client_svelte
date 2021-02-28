<script>
  import { Router, Route, Link } from "svelte-routing";
  import Header from "./component/Header.svelte";
  import Footer from "./component/Footer.svelte";
  import Home from "./component/Home.svelte";
  import Login from "./component/Login.svelte";
  import Admin from "./component/Admin.svelte";
  import Game from "./component/Game.svelte";
  import Settings from "./component/Settings.svelte";
  import { onMount } from "svelte";
  import io from "socket.io-client";

  
  onMount(async () => {
    // Quick fix for setting body margin to 0 
    document.getElementsByTagName("body")[0].style.margin = "0px";
    // The header has a linkholder element and a empy dummy element on the other side of the logo
    const orig = document.getElementById("orig");
    const dummy = document.getElementById("dummy");
    // Set the dummy element to be of same width as the linkholder so they are flexed correctly
    dummy.style.width = orig.offsetWidth + "px";
  });

  // Global variables which can be passed around to child components
  // the url variable is needed for the Router
  export let url = "";
  // Creates a socket object which will handle all the socket communications
  const socket = io("https://tlk-qoiz-server-prod.herokuapp.com/");
  // Global user variable
  let user = {};

  //socket.on("connect", (d) => {
  //  console.log("socket connected");
  //});
</script>

<Settings {socket}></Settings>

<main id="app">
  <Router {url}>
    <Header />
    <div>
      <Route path="/"><Home {user} /></Route>
      <Route path="login"><Login bind:user /></Route>
      <Route path="admin"><Admin {socket} {user} /></Route>
      <Route path="game"><Game {socket} {user} /></Route>
    </div>
  </Router>
  <Footer />
</main>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Overpass:ital,wght@0,100;0,200;0,300;0,400;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,600;1,700;1,800;1,900&family=Roboto:wght@100&display=swap");
  #app {
    font-family: Overpass, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-bottom: 10em;
  }
  
</style>
