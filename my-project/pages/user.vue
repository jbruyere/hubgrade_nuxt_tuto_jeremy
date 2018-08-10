<template>
  <div>
    <h1>Find a user (by username) :</h1>
	<v-form v-model="valid">
		<v-text-field background-color="grey" v-model="usernamesearch" label="username"
			:rules="usernameRule" color="blue" required solo>
		</v-text-field>
		<v-btn :disabled="!valid" color="green"
		block @click="search" fab>Search</v-btn>
	</v-form>
	<v-btn block="" to="/">Home</v-btn>
	<v-alert :value="alert" type="error"> User not found </v-alert>	
		<div v-if="profil && !alert">
			<h2><br /><br />User : {{ username }}<br /><br /></h2>
			<p><strong>Pseudo: </strong>{{ pseudo }}</p>
			<p><strong>Email: </strong>{{ email }}</p>
			<p><strong>Phone: </strong>{{ phone }}</p>
			<p><strong>City: </strong>{{ city }}</p>
			<p><strong>Zipcode: </strong>{{ zipcode }}</p>
			<p><strong>Bio: </strong>{{ bio }}</p>
		</div>
    <nuxt/>
  </div>
</template>
<script>
  export default {
    layout: 'layout',
    data: () => ({
      valid: false,
      usernamesearch: '',
      profil: false,
      alert: false,
      err: true,
      username: '',
      pseudo: '',
      email: '',
      phone: '',
      city: '',
      zipcode: '',
      bio: '',
      usernameRule: [
        v => !!v || 'Username is required'
      ]
    }),
    methods: {
      search () {
        this.err = true
        this.$axios.post('http://localhost:8000/profil', {
          username: this.usernamesearch
        }).then(res => {
          this.username = res.data.username
          this.pseudo = res.data.pseudo
          this.email = res.data.email
          this.phone = res.data.phone
          this.city = res.data.city
          this.zipcode = res.data.zipcode
          this.bio = res.data.bio
          this.profil = true
          this.alert = false
          this.err = false
        })
        if (this.err) {
          this.alert = true
        }
      }
    }
  }
</script>