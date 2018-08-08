<template>
  <div id="app">
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
	</v-form>
    <nuxt/> <!-- appele layout, ici layout default.vue-->
  </div>
</template>

<script>
  export default {
    layout: 'layout',
    auth: false,
    data: () => ({
      valid: false,
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
        this.$auth.loginWith('local', {
          data: {username: this.username,
            password: this.password}
        }).then((result) => console.log(result))
          .catch((err) => console.log(err))
      }
    }
  }
</script>