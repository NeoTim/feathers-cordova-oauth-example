<template>
  <div id="app">
    <template v-if="logged">
      <p>Your are logged in.</p>
      <button @click='logout()'>logout</button>
    </template>
    <a
      v-else
      href="http://localhost:3030/auth/github"
    >Login With GitHub</a>
  </div>
</template>

<script>
import feathers from 'feathers'
import auth from 'feathers-authentication-client'
import socketio from 'feathers-socketio'
import io from 'socket.io-client'

const socket = io('http://localhost:3030/', { transports: ['websocket'] })

const feathersClient = feathers()
  .configure(socketio(socket))
  .configure(auth({ storage: window.localStorage }))

export default {
  name: 'app',

  data () {
    return {
      logged: false
    }
  },

  methods: {
    logout () {
      feathersClient.logout()
        .then(() => {
          console.info('Feathers Client has logged out')
          this.logged = false
        })
    }
  },

  created () {
    feathersClient.authenticate()
      .then(response => {
        console.info('Feathers Client has Authenticated with the JWT access token!')
        console.log(response)
        this.logged = true
      })
      .catch(error => {
        console.info('We have not logged in with OAuth, yet.  This means there\'s no cookie storing the accessToken.  As a result, feathersClient.authenticate() failed.')
        console.log(error)
      })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
