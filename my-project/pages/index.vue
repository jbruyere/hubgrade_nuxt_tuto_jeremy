<template>
  <!--<v-layout column justify-center align-center>
    <v-flex xs12 sm8 md6>
      <div class="text-xs-center">
        <img src="/v.png" alt="Vuetify.js" class="mb-5" />
      </div>
      <v-card>
        <v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>
        <v-card-text>
          <p>Vuetify is a progressive Material Design component framework for Vue.js. It was designed to empower developers to create amazing applications.</p>
          <p>For more information on Vuetify, check out the <a href="https://vuetifyjs.com" target="_blank">documentation</a>.</p>
          <p>If you have questions, please join the official <a href="https://chat.vuetifyjs.com/" target="_blank" title="chat">discord</a>.</p>
          <p>Find a bug? Report it on the github <a href="https://github.com/vuetifyjs/vuetify/issues" target="_blank" title="contribute">issue board</a>.</p>
          <p>Thank you for developing with Vuetify and I look forward to bringing more exciting features in the future.</p>
          <div class="text-xs-right">
            <em><small>&mdash; John Leider</small></em>
          </div>
          <hr class="my-3">
          <a href="https://nuxtjs.org/" target="_blank">Nuxt Documentation</a>
          <br>
          <a href="https://github.com/nuxt/nuxt.js" target="_blank">Nuxt GitHub</a>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" flat nuxt to="/inspire">Continue</v-btn>
        </v-card-actions>
      </v-card>
    </v-flex>
  </v-layout> -->
  <div id="app">
    <h1>Welcome to vuetify</h1>
    <v-btn block="" to="/hello-world">Hello world</v-btn>
    <v-btn block="" to="/user">Find user</v-btn>
    <v-btn block="" color="green" @click="click">Account </v-btn>
    <v-form v-if="account">
      <v-btn block="" to="/login" v-if="!logged">Login</v-btn>
      <v-btn block="" to="/register" v-if="!logged">Register</v-btn>
      <v-btn block="" v-if="logged" to="/profil">Profil</v-btn>
      <v-btn block="" v-if="logged" to="/post">Posts</v-btn>
    </v-form>
    <v-form>
      <v-btn color="red" block="" v-if="logged" @click="logout">Logout</v-btn>
    </v-form>
    <nuxt/> 
  </div>
</template>

<script>
  export default {
    layout: 'layout',
    name: 'app',
    data: () => ({
      logged: false,
      account: false
    }),

    methods: {
      click () {
        this.$axios.get('http://localhost:8000/logged'
        ).then(res => {
          this.account = !this.account
          if (res.data.log === 'true') {
            this.logged = true
          } else {
            this.logged = false
          }
        })
      },
      logout () {
        this.$axios.delete('http://localhost:8000/logout'
        ).then(res => {
          this.account = false
          this.logged = false
        }).catch(err => {
          console.log(err)
        })
      }
    }
  }
</script>