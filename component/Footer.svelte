<script>

</script>

<main>
  <div id="foot">
    <div>
      <router-link to="/"
        ><img src="../assets/qoiz_logo_transparent.png" id="logo"
      /></router-link>
    </div>
    <div>
      <router-link class="link" to="/contact">Contact</router-link>
      <a href="https://www.tlk.fi/" class="link"> TLK </a>
    </div>
  </div>
</main>

<style>
  #foot {
    background-color: steelblue;
    color: darkgoldenrod;
    position: fixed;
    bottom: 0;
    width: 100%;
    left: 0;
    height: 5vh;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .side {
    margin: 1em;
  }
  .link {
    margin: 1em;
    color: darkgoldenrod;
    font-weight: bold;
    text-decoration: none;
  }
  
  .link:hover {
    color: rgb(223, 185, 90);
  }
  #logo {
    margin-left: 2vh;
    max-height: 3vh;
  }
  </style>