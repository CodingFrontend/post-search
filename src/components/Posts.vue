<template>
  <div class="posts pt-5 bg-light">
    <div class="posts__container container">
      <div class="posts__search mb-5 d-flex justify-content-center">
        <div class="posts__search-input border d-inline-flex rounded bg-white">
          <div class="posts__search-input-icon">
            <img src="../assets/img/icons/search.svg" />
          </div>
          <input
            v-model="search"
            type="search"
            autocomplete="off"
            placeholder="Filter by author..."
          />
        </div>
      </div>
      <ul class="posts__list card-columns">
        <li v-for="post in postList" :key="post.id" class="card">
          <div class="card-inner">
            <div class="card-body">
              <p class="card-title text-info fw-bold">
                {{ post.title }}
              </p>
              <p class="card-text">{{ post.body }}</p>
              <p class="card-text">
                <small class="text-muted">{{ post.author }}</small>
              </p>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Posts",
  data() {
    return {
      postsData: [],
      authorsData: [],
      search: "",
      posts: [],
    };
  },
  mounted() {
    const urlPosts = "http://jsonplaceholder.typicode.com/posts";
    const urlAuthors = "http://jsonplaceholder.typicode.com/users";

    const requestPosts = axios.get(urlPosts);
    const requestAuthors = axios.get(urlAuthors);

    axios
      .all([requestPosts, requestAuthors])
      .then(
        axios.spread((...responses) => {
          this.postsData = responses[0].data;
          this.authorsData = responses[1].data;

          this.posts = this.postsData.map((post) => {
            const index = this.authorsData.findIndex(
              (el) => el.id === post.userId
            );
            if (index === -1) {
              return post;
            }

            return Object.assign(post, {
              author: this.authorsData[index].name,
            });
          });
        })
      )
      .catch((errors) => {
        console.error(errors);
      });
  },

  computed: {
    postList() {
      return this.posts.filter((post) => {
        return post.author.toLowerCase().includes(this.search.toLowerCase());
      });
    },
  },
};
</script>

<style lang="scss">
@import "./posts.scss";
</style>