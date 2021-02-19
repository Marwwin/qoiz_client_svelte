<script>
  import {Link} from "svelte-routing";
  document.getElementsByTagName("body")[0].style.padding = 0;
</script>

<main>
  <div id="head">
    <div class="side" id="dummy"></div>
    <div>
        <img src="./assets/qoiz_logo_transparent.png" id="logo" />
    </div>
<nav id="orig">
  <Link to="/" ><span class="link">Home</span></Link>
  <Link to="login" > <span class="link">Login/Signup </span></Link>
</nav>
  </div>
</main>
<style>

  #head {
  background-color: steelblue;
  color: darkgoldenrod;
  height: 12vh;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.side {
  margin: 1em;
}
.link {
  margin: 1em;
  color: rgb(223, 185, 90);
  font-weight: bold;
  font-size: x-large;
  text-decoration: none;
}
.link:hover {
  margin: 1em;
  color: darkgoldenrod;
  font-weight: bold;
  font-size: x-large;
}
#logo {
  max-height: 10vh;
}
</style>