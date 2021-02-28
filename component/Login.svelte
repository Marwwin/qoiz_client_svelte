<script>
  
  import { Link } from "svelte-routing";

  export let user = {};

  let userLoggedIn = false;
  let username = "";
  let password = "";

  // Try to login the user
  function loginUser() {
    fetch("https://tlk-qoiz-server-prod.herokuapp.com/users/login/", {
      method: "POST",
      headers: {
        Accept: "application/json",
        "Content-Type": "application/json",
      },
      // Set username and password in body
      body: JSON.stringify({
        username: username,
        password: password,
      }),
    })
      .then((res) => res.json())
      .then((res) => {
        if (
          res.message != "Authentication failed (check your email and password)"
        ) {
          user = res;
          userLoggedIn = true;
        }
      });
  }
  // Quick fix for getting back to the home page.
  // This function is run if the user is logged in
 function finc(e){
   setTimeout(d => e.click(),100)
  // e.click()
 }
</script>

{#if !userLoggedIn}
  <div id="login">
    <div class="outer">
      <h2>Log in</h2>
      <p>Please log in with your username and password</p>
      <br />
      <div class="box">
        <form action="login">
          Username:
          <input
            type="text"
            class="input-field"
            id="username"
            bind:value={username}
          /><br />
          Password:
          <input
            type="text"
            class="input-field"
            id="password"
            bind:value={password}
          /><br />
          <br />
          <div>
            <input
              on:click={loginUser}
              type="button"
              id="login-btn"
              value="Log in"
            />
          </div>
        </form>
      </div>
    </div>
    <p><a href="../">Forgot your password?</a></p>
    <p><a href="../register">Register</a></p>
  </div>
{:else}
  <Link to="/"><button id="link_to_main" use:finc>You are now logged in Click Me</button></Link>
{/if}

<style>
  #login {
    margin-top: 5vh;
  }

  #login-btn {
    display: inline-flex;
    justify-content: center;
    align-items: center;
    padding: 0 20px;
    width: 100%;
    height: 60px;
    background-color: #333333;
    border-radius: 10px;
    color: rgb(223, 185, 90);
    font-weight: 400;
  }

  .outer {
    padding-top: 4%;
    list-style-type: none;
    text-align: center;
    margin: 0;
    padding: 0;
  }
  .box {
    display: inline-flex;
    text-align: center;
    padding: 4%;
    background-color: white;
    border: 1px solid #e6e6e6;
    border-style: solid;
    border-radius: 4%;
  }

  .input-field {
    width: 100%;
    position: relative;
    background-color: #f7f7f7;
    border: 1px solid #e6e6e6;
    border-radius: 10px;
  }
  #link_to_main{
    display: none;
  }
</style>
