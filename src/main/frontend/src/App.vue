<template>
  <div id="app">
    <h1>Witaj w systemie do zapisów na zajęcia!</h1>

    <div v-if="authenticatedUsername">
      <UserPanel :username="authenticatedUsername" @logout="logMeOut()"></UserPanel>
      <MeetingsPage :username="authenticatedUsername"></MeetingsPage>
    </div>

    <div v-else>
      <button @click="registering=false" :class="registering ? 'button-outline' : ''">Logowanie.</button>
      <button @click="registering=true" :class="!registering ? 'button-outline' : ''">>Rejestracja.</button>

      <LoginForm v-if="!registering" @login="(user) => logMeIn(user)"></LoginForm>
      <LoginForm v-else @login="(user)=> register(user)" button-label="Zaloz konto"></LoginForm>


      <div v-if="message" class = "alert">{{message}}</div>
       <h> </h>

    </div>
  </div>
</template>

<script>
import "milligram";
import LoginForm from "./LoginForm";
import UserPanel from "./UserPanel";
import MeetingsPage from "./meetings/MeetingsPage";
import RegisterForm from "@/meetings/RegisterForm";
import axios, {isAxiosError} from "axios";

export default {
  components: {LoginForm, MeetingsPage, UserPanel},
  data() {
    return {
      authenticatedUsername: '',
      registering: false,
      message:""
    }
  },
  methods: {
    logMeIn(user) {

      axios.post('/api/tokens', user)
          .then(response => {
            // alert("Account not added. This user already exists")
            this.authenticatedUsername = user.login;
            const token = response.data.token;
            axios.defaults.headers.common['Authorization'] = 'Bearer ' + token;
            axios.get('/api/meetings').then(response => console.log(response.data));

          })
          .catch(response => {
            // alert("unable to add")
            this.message="nie udalo sie zalogowac"

          });

        }


    ,
    logMeOut() {
      this.authenticatedUsername = '';
      delete axios.defaults.headers.common.Authorization;
    },
    register(user) {
      axios.post('/api/participants', user)
          .then(response => {
            // alert("Account not added. This user already exists")
            this.message = "Twoje konto zostalo zalozone"

          })
          .catch(response => {
            // alert("unable to add")
            this.message = "Twoje konto nie zostalo zalozone"
          });
    }

}}
</script>

<style>
#app {
  max-width: 1000px;
  margin: 0 auto;
}
.alert{
  border: 2px green solid;
}

.alert-red{
  border: 2px red;
}
</style>
