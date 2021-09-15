<template>
  <div id="app">
    <Posts />
  </div>
</template>

<script>
import "bootstrap/dist/css/bootstrap.min.css";
import Posts from "./components/Posts.vue";

export default {
  name: "App",
  components: {
    Posts,
  },
};
</script>

<style lang="scss">
#app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>
