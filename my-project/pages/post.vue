<template>
  <div>
	<v-btn block="" to="/">Home</v-btn>
	<v-form v-model="valid">
		<v-textarea v-model="content" label="Content for new post :"
			:rules="postRule"
		color="yellow" required>
		</v-textarea>
		<v-btn :disabled="!valid" color="green"
		@click="submit">Create post</v-btn>
	</v-form>
	<v-alert :value="ok" type="success"> Post created</v-alert>
	<v-alert :value="okdel" type="success"> Post deleted</v-alert>
    <v-btn @click="getposts">See all posts</v-btn>
	<div v-if="done" v-for="p in posts">
		<p><strong> From user {{ p.user }}: </strong>
		<br />{{ p.content }}<br />
		{{ p.date.date }}</p>
		<v-btn v-if="p.delete" @click="deletepost(p.id)">Delete</v-btn>
		<p><br /><br /></p>
	</div>
    <nuxt/>
  </div>
</template>
<script>
  export default {
    layout: 'layout',
    data: () => ({
      posts: {},
      valid: false,
      ok: false,
      okdel: false,
      username: '',
      id: 0,
      content: '',
      done: false,
      postRule: [
        v => !!v || 'Content is required',
        v => (v && v.length >= 5) || 'Content size must be between 5 and 200 characters',
        v => (v && v.length <= 200) || 'Content size must be between 5 and 200 characters'
      ]
    }),
    methods: {
      getposts () {
        this.okdel = false
        this.ok = false
        this.$axios.get('http://localhost:8000/user').then(res => {
          this.username = res.data.username
          this.$axios.post('http://localhost:8000/profil', {
            username: this.username
          }).then(res => {
            this.id = res.data.id
            this.$axios.get('http://localhost:8000/post').then(res => {
              this.posts = res.data
              for (var i = 0; this.posts[i] != null; i++) {
                if (this.posts[i].user === this.id) {
                  this.posts[i].delete = true
                }
              }
              this.done = true
              this.ok = false
            })
          })
        })
      },
      submit () {
        this.okdel = false
        this.$axios.post('http://localhost:8000/post/create', {
          content: this.content
        }).then(res => {
          this.content = ''
          this.getposts()
          this.ok = true
          this.done = true
        })
      },
      deletepost (id) {
        this.$axios.delete('http://localhost:8000/post/delete/' + id).then(res => {
          this.done = false
          this.okdel = true
        })
      }
    }
  }
</script>