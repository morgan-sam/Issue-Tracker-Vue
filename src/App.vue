<template>
  <div class="app">
    <div class="horizontal-container">
      <form>
        <input v-model="search" type="text" />
      </form>
      <button v-on:click="getGithubResults">Search</button>
    </div>
    <div class="horizontal-container">
      <Grid
        v-bind:results="results"
        v-bind:showIssues="showIssues"
        v-bind:setSelectedRepo="setSelectedRepo"
      />
      <IssueList
        v-bind:issues="issues"
        v-bind:issuePage="issuePage"
        v-bind:changeIssuePage="changeIssuePage"
      />
    </div>
  </div>
</template>

<script>
import IssueList from "./components/IssueList.vue";
import Grid from "./components/Grid.vue";

export default {
  name: "App",
  data: () => ({
    issues: [],
    results: [],
    selectedRepo: null,
    issuePage: 1,
    search: "",
  }),
  components: {
    IssueList,
    Grid,
  },
  methods: {
    getGithubResults: async function () {
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const req = await fetch(
        `https://api.github.com/search/repositories?q=${this.search}&per_page=20&page=1&sort=stars&order=desc`,
        {
          headers: {
            Authorization: `token ${authToken}`,
          },
        }
      );
      const data = await req.json();
      this.results = data.items;
    },
    showIssues: async function (page) {
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const req = await fetch(
        `${this.selectedRepo.url}/issues?per_page=20&page=${page}`,
        {
          headers: {
            Authorization: `token ${authToken}`,
          },
        }
      );
      const data = await req.json();
      this.issues = data;
    },
    changeIssuePage: async function (diff) {
      let newPage = this.issuePage + diff;
      if (newPage < 1) newPage = 1;
      await this.showIssues(newPage);
      this.issuePage = newPage;
    },
    setSelectedRepo: function (repo) {
      this.selectedRepo = repo;
    },
  },
  watch: {
    selectedRepo: async function () {
      await this.showIssues();
      this.issuePage = 1;
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
