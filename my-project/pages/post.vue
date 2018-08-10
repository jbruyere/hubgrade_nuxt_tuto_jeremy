<template>
  <div>
	<v-btn block="" to="/">Home</v-btn>
    <v-btn @click="getposts">See all posts</v-btn>
	<div v-if="done" v-for="p in posts">
		<h3><strong> From user {{ p.user }} : </strong>
		<br />{{ p.content }}<br />
		{{ p.date.date }}<br /><br /></h3>
	</div>
    <nuxt/>
  </div>
</template>
<script>
  export default {
    layout: 'layout',
    data: () => ({
      posts: {},
      done: false
    }),
    methods: {
      getposts () {
        this.done = false
        this.$axios.get('http://localhost:8000/post').then(res => {
          this.posts = res.data
          this.done = true
        })
      }
    }
  }
</script>