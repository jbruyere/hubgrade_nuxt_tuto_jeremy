<template>
  <div id="app">
    <h1>Profil</h1>
	<v-form v-model="valid" v-if="logged">
		<v-btn v-if="!profil" color="green" @click="submit">See profil</v-btn>
		<v-btn v-if="profil" color="green" @click="profil=!profil">Hide</v-btn>
		<div v-if="profil">
			<p><strong>Username: </strong>{{ username }}</p>
			<p><strong>Pseudo: </strong>{{ pseudo }}</p>
			<p><strong>Email: </strong>{{ email }}</p>
			<p><strong>Id: </strong>{{ id }}</p>
			<p><strong>Phone: </strong>{{ phone }}</p>
			<p><strong>City: </strong>{{ city }}</p>
			<p><strong>Zipcode: </strong>{{ zipcode }}</p>
			<p><strong>Bio: </strong>{{ bio }}</p>
		</div>
	</v-form>
	<v-btn to="/">Home</v-btn>
    <nuxt/>
  </div>
</template>

<script>
  export default {
    layout: 'layout',
    data: () => ({
      valid: true,
      profil: false,
      username: '',
      pseudo: '',
      id: '',
      email: '',
      city: '',
      phone: '',
      zipcode: '',
      bio: ''
    }),
    computed: {
      logged () {
        return this.$auth.loggedIn
      }
    },
    methods: {
      submit () {
        this.$axios.put('http://localhost:8000/user/profil').then(res => {
          this.profil = true
          this.email = res.data.email
          this.username = res.data.username
          this.pseudo = res.data.pseudo
          this.bio = res.data.bio
          this.zipcode = res.data.zipcode
          this.id = res.data.id
          this.phone = res.data.phone
          this.city = res.data.city
        }).catch(err => {
          console.log(err)
        })
      }
    }
  }
</script>