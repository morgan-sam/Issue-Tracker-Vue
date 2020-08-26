<template>
  <div id="app">
    <IssueList v-bind:issues="issues" />
  </div>
</template>

<script>
import IssueList from "./components/IssueList.vue";

export default {
  name: "App",
  data: () => {
    return {
      issues: [{ name: "Bug" }, { name: "Typo" }, { name: "Missing Feature" }],
    };
  },
  components: {
    IssueList,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
