<template>
  <div id="app">
    <header>
      <h1>Vue.js SPA</h1>
    </header>
    <main>
      <aside class="sidebar">
        <router-link 
          v-for="post in posts"
          active-class="is-active"
          class="link"
          :to="{ name: 'post', params: { id: post.id } }">
          {{post.id}}. {{post.title}}
        </router-link>
      </aside>
      <div class="content">
        <router-view></router-view>
      </div>
    </main>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  props: ['id'],
  data() {
    return {
      posts: null,
      endpoint:'https://jsonplaceholder.typicode.com/posts/',
    }
  },

  created() {
    this.getAllPosts();
    this.getPost(this.id);
  },

  methods: {
    getAllPosts() {
      axios.get(this.endpoint)
      .then(response => {  
        this.posts=response.data;
      })
      .catch(error => {
        console.log('-----error-------');
        console.log(error);
      })
    },

    getPost(id) {
      axios(this.endpoint + id)
        .then(response => {
          this.post = response.data
        })
        .catch(error => {
          console.log(error)
        })
    },
  },

  watch: {
    '$route'() {
      this.getPost(this.id);
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
