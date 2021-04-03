<template>
  <div id="app">
    <h2>Search manuscripts</h2>
    <SearchManuscripts></SearchManuscripts>
  </div>
</template>

<script>
import SearchManuscripts from "./components/SearchManuscripts";

export default {
  name: "App",
  components: {
    SearchManuscripts,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
