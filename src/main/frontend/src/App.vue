<template>
  <div id="app">
    <h1>
      <img src="./assets/logo.svg" alt="Enroller" class="logo">
      System do zapisów na zajęcia
    </h1>
    <div v-if="authenticatedUsername">
      <h2>Witaj {{ authenticatedUsername }}!
        <a @click="logout()" class="float-right  button-outline button">Wyloguj</a>
      </h2>
      <meetings-page :username="authenticatedUsername"></meetings-page>
    </div>
    <div v-else>
      <button @click="registering=false" :class="!registering ? '' : 'button-clear'">Zaloguj sie</button>
      <button @click="registering=true" :class="registering ? '' : 'button-clear'">Zarejestruj sie</button>
      <div :class="isErrorMessage ? 'red' : 'green'">{{message}}</div>
      <login-form @login="login($event)" v-if="registering === false"></login-form>
      <login-form @login="register($event)" button-label="Zarejestruj sie" v-else></login-form>
    </div>
  </div>
</template>

<script>
    import "milligram";
    import LoginForm from "./LoginForm";
    import MeetingsPage from "./meetings/MeetingsPage";

    export default {
        components: {LoginForm, MeetingsPage},
        data() {
            return {
                registering: false,
                authenticatedUsername: "",
                message: '',
                isErrorMessage: false,
            };
        },
        methods: {
            login(user) {
                this.authenticatedUsername = user.login;
            },
            logout() {
                this.authenticatedUsername = '';
            },
            register(user){
               this.$http.post('participants', user)
              .then(response => {
              this.message = "udalo sie";
              this.isErrorMessage = false;
               })
              .catch(response => {
              this.message = "nie udalo sie";
              this.isErrorMessage = true;
             });

            }
        }
    };
</script>

<style>
  #app {
    max-width: 1000px;
    margin: 0 auto;
  }

  .logo {
    vertical-align: middle;
  }

  .red{
    background: red;
  }
  .green{
    background: green;
  }
</style>

