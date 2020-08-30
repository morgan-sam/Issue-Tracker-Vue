<template>
  <div id="app">
    <IssueList v-bind:issues="issues" />
    <Grid v-bind:results="results" />
    <button v-on:click="newIssueList">Randomise Issues</button>
    <button v-on:click="getGithubResults">getGithubResults</button>
  </div>
</template>

<script>
import IssueList from "./components/IssueList.vue";
import Grid from "./components/Grid.vue";

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
    results: [
      { name: "Alarm App", stars: 1241 },
      { name: "Path Finder", stars: 3545 },
    ],
  }),
  components: {
    IssueList,
    Grid,
  },
  methods: {
    newIssueList: async function () {
      const newIssues = await genIssueList(6);
      this.issues = newIssues;
    },
    getGithubResults: async function () {
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const req = await fetch(
        `https://api.github.com/search/repositories?q=react&sort=stars&order=desc`,
        {
          headers: {
            Authorization: `token ${authToken}`,
          },
        }
      );
      const data = await req.json();
      this.results = data.items;
      console.log(data.items);
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
