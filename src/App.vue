<template>
  <div class="app">
    <div class="horizontal-container">
      <Grid v-bind:results="results" v-bind:showIssues="showIssues" />
      <IssueList v-bind:issues="issues" />
    </div>
    <div class="horizontal-container">
      <button v-on:click="getGithubResults">getGithubResults</button>
    </div>
  </div>
</template>

<script>
import IssueList from "./components/IssueList.vue";
import Grid from "./components/Grid.vue";

export default {
  name: "App",
  data: () => ({
    issues: [{ name: "Bug" }, { name: "Typo" }, { name: "Missing Feature" }],
    results: [
      { name: "Alarm App", stargazers_count: 1241 },
      { name: "Path Finder", stargazers_count: 3545 },
    ],
  }),
  components: {
    IssueList,
    Grid,
  },
  methods: {
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
    },
    showIssues: async function (repo) {
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const req = await fetch(`${repo.url}/issues?per_page=50`, {
        headers: {
          Authorization: `token ${authToken}`,
        },
      });
      const data = await req.json();
      this.issues = data;
    },
  },
};
</script>

<style>
.app {
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
  margin: 0.5rem;
}
.horizontal-container {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
</style>
