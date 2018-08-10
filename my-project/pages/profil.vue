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
	<v-btn color="green" @click="edition = !edition">Edit</v-btn>
	<v-form v-if="edition" v-model="valid">
		<v-text-field v-model="pseudofield" label="new pseudo" color="yellow">
		</v-text-field>
		<v-text-field v-model="phonefield" label="new phone" color="yellow">
		</v-text-field>
		<v-text-field v-model="cityfield" label="new city" color="yellow">
		</v-text-field>
		<v-text-field v-model="zipcodefield" label="new zipcode" color="yellow">
		</v-text-field>
		<v-textarea v-model="biofield" label="new bio" color="yellow">
		</v-textarea>
		<v-btn color="green"
		block @click="update" fab>Update profil</v-btn>
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
      edition: false,
      username: '',
      pseudo: '',
      id: '',
      email: '',
      city: '',
      phone: '',
      zipcode: '',
      bio: '',
      pseudofield: '',
      cityfield: '',
      phonefield: '',
      zipcodefield: '',
      biofield: '',
      keys: ['phone', 'city', 'zipcode', 'bio', 'pseudo', null],
      values: [],
      infos: {}
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
      },
      update () {
        this.values = [this.phonefield, this.cityfield, this.zipcodefield, this.biofield, this.pseudofield]
        for (var i = 0; this.keys[i] != null; i++) {
          if (this.values[i] != null) {
            this.infos[this.keys[i]] = this.values[i]
          }
        }
        this.$axios.put(
          'http://localhost:8000/user/profil',
          this.infos).then(res => {
          this.pseudo = res.data.pseudo
          this.bio = res.data.bio
          this.zipcode = res.data.zipcode
          this.phone = res.data.phone
          this.city = res.data.city
        }).catch(err => {
          console.log(err)
        })
      }
    }
  }
</script>