<template>
  <div id="app">
    <p id="p"></p>
    <h1>Login</h1>
	<v-form v-model="valid">
		<v-text-field v-model="username" label="username"
			:rules="usernameRule" color="yellow" required>
		</v-text-field>
		<v-text-field v-model="password" label="password"
			:rules="passwordRule" type="password"
		color="yellow" required>
		</v-text-field>
		<v-btn :disabled="!valid" color="green"
		block @click="submit" fab>Submit</v-btn>
    <!--<p v-if="ok"> {{ user }} is connected</p>
    <p v-else>Not connected</p>-->
	</v-form>
    <v-btn block="" to="/">Home</v-btn>
    <v-alert :value="alert" type="error"> Invalid Username/password</v-alert>
    <v-alert :value="!alert && ok" type="success"> {{ user }} is connected</v-alert>
    <nuxt/>
  </div>
</template>

<script>
  export default {
    layout: 'layout',
    auth: false,
    data: () => ({
      ok: false,
      user: '',
      valid: false,
      alert: false,
      username: '',
      usernameRule: [
        v => !!v || 'Username is required',
        v => (v && v.length >= 3) || 'Username must be more than 2 characters'
      ],
      password: '',
      passwordRule: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 3) || 'Password must be more than 2 characters'
      ]
    }),

    methods: {
      submit () {
        console.log(this.$auth)
        this.$auth.loginWith('local', {
          data: {username: this.username,
            password: this.password}
        }).then(res => {
          this.user = this.username
          this.ok = true
          this.alert = false
        })
        if (this.ok === false) {
          this.alert = true
        }
      }
    }
  }
</script>