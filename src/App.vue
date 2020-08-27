<template>
  <div id="app">
    <IssueList v-bind:issues="issues" />
    <button v-on:click="newIssueList" />
  </div>
</template>

<script>
import IssueList from "./components/IssueList.vue";

const randomString = () => Math.random().toString(36).substring(7);
const genIssueList = (num) => [...Array(num)].map(() => randomString());

export default {
  name: "App",
  data: () => ({
    issues: [{ name: "Bug" }, { name: "Typo" }, { name: "Missing Feature" }],
  }),
  components: {
    IssueList,
  },
  methods: {
    newIssueList: () => console.log(genIssueList(5)),
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
  display: flex;
  justify-content: center;
}
</style>
