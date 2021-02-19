<script>
  let username = "";
  let password = "";
  //fetch("http://desolate-lowlands-50870.herokuapp.com/users/signup/", {
  fetch("https://tlk-qoiz-server-prod.herokuapp.com/users/signup/", {
    method: "POST",
    headers: {
      Accept: "application/json",
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      username: username,
      password: password,
    }),
  }).then((res) => {
    if (res.statusText == "Created") {
      //  alert("User " + this.username + " Created");
    }
  });
</script>

<div id="register">
  <div class="outer">
    <h2>Register</h2>
    <p>Please fill out the following information and start to play!</p>
    <br />
    <div class="box">
      <form action="register">
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
          <input type="button" id="register-btn" value="Register" />
        </div>
      </form>
    </div>
  </div>
</div>

<style>
  #register {
    margin-top: 5vh;
  }

  #register-btn {
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
</style>
