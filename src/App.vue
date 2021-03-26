<template>
  <div id="app">
    <Layout msg="Welcome to Your Vue.js App" />
  </div>
</template>

<script>
import Layout from "./pages/Layout.vue";

export default {
  name: "App",
  components: {
    Layout,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
