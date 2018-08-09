<template>
  <div id="app">
    <h1>Register</h1>
	<v-form v-model="valid">
		<v-text-field v-model="username" label="username"
			:rules="usernameRule" color="yellow" required>
		</v-text-field>
		<v-text-field v-model="pseudo" label="pseudo"
			:rules="pseudoRule" color="yellow" required>
		</v-text-field>
		<v-text-field v-model="email" label="email"
			:rules="emailRule" type="email" 
			color="yellow" required>
		</v-text-field>
		<v-text-field v-model="password" label="password"
			:rules="passwordRule" type="password"
		color="yellow" required>
		</v-text-field>
		<v-btn :disabled="!valid" color="green"
		block @click="submit" fab>Submit</v-btn>
	</v-form>
	<v-btn block="" to="/">Home</v-btn>
    <nuxt/>
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
      pseudo: '',
      pseudoRule: [
        v => !!v || 'Pseudo is required',
        v => (v && v.length >= 3) || 'Pseudo must be more than 2 characters'
      ],
      email: '',
      emailRule: [
        v => !!v || 'Email is required',
        v => /.+@.+/.test(v) || 'Email must be valid'
      ],
      password: '',
      passwordRule: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 3) || 'Password must be more than 2 characters'
      ]
    }),

    methods: {
      submit () {
        this.$axios.post('http://localhost:8000/register', {
          username: this.username,
          pseudo: this.pseudo,
          email: this.email,
          password: this.password
        }).then(res => {
          this.$router.push('/login')
        }).catch(err => {
          console.log(err)
        })
      }
    }
  }
</script>