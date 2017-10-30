<template>
  <div id="app">
    <template v-if="logged">
      <p>Your are logged in.</p>
      <button @click='logout()'>logout</button>
    </template>
    <template v-else>
      <button @click='cordovaOauthLogin()'>Login With GitHub (for cordova)</button>
      <hr>
      <a href="http://localhost:3030/auth/github">Login With GitHub (for browsers)</a>

    </template>

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
    },

    login () {
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
    },

    cordovaOauthLogin () {
      // Open a new InAppBrowser for linkedin oauth login
      const browser = window.cordova.InAppBrowser.open(
        'http://localhost:3030/auth/github',
        '_blank',
        'location=no,clearsessioncache=yes,clearcache=yes'
      )

      // Starts event to detect when the login has finished
      // and the feathers cookie is ready
      browser.addEventListener('loadstop', event => {
        // Detect the successRedirect from feathers
        if (event.url === 'http://localhost:8080/') {
          browser.executeScript(
            { code: 'document.cookie.valueOf("feathers-jwt")' },
            async cookie => {
              // Extract and set feathers-jwt token from cookie
              const token = `${cookie}`.slice(13)
              localStorage.setItem('feathers-jwt', token)

              // Close the InAppBrowser
              browser.close()

              // Login after token is ready on the localStorage
              this.login()
            }
          )
        }
      })
    }
  },

  created () {
    this.login()
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
