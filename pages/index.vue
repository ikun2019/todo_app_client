<template>
  <div>
    <h1>Todo</h1>
    <div>
      <input type="text" name="text" id="text" v-model="text">
      <button type="submit" @click.prevent="createTodo">投稿</button>
    </div>
    <ul>
      <li v-for="(post, index) in posts" :key="index">{{ post.text }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      text: '',
      posts: []
    }
  },
  mounted() {
    this.fetchPosts();
  },
  methods: {
    async fetchPosts() {
      try {
        const response = await this.$axios.$get(`${process.env.API_URL}/posts`);
        this.posts = response.posts;
      } catch (err) {
        console.error(err);
      }
    },
    async createTodo() {
      try {
        const data = new FormData();
        data.append('name', 'ikun');
        data.append('text', this.text);
        let response = await this.$axios.$post(`${process.env.API_URL}/posts`, data);
        this.text = '';
        this.fetchPosts();
        this.$router.push('/');
      } catch (err) {
        console.error(err);
      }
    }
  }
}

</script>
