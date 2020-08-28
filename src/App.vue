<template>
  <div id="app">
    <IssueList v-bind:issues="issues" />
    <button v-on:click="newIssueList">Randomise Issues</button>
  </div>
</template>

<script>
import IssueList from "./components/IssueList.vue";

const capitalise = (str) => str.charAt(0).toUpperCase() + str.slice(1);
const randomWords = async () => {
  const req = await fetch(
    "https://random-word-api.herokuapp.com/word?number=2"
  );
  let text = await req.json();
  return capitalise(text.join(" "));
};
const genIssueList = async (num) => {
  return await Promise.all(
    [...Array(num)].map(async () => ({ name: await randomWords() }))
  );
};
export default {
  name: "App",
  data: () => ({
    issues: [{ name: "Bug" }, { name: "Typo" }, { name: "Missing Feature" }],
  }),
  components: {
    IssueList,
  },
  methods: {
    newIssueList: async function () {
      const newIssues = await genIssueList(6);
      this.issues = newIssues;
    },
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
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
button {
  width: fit-content;
}
</style>
