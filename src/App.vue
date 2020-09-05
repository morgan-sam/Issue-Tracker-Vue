<template>
  <div class="app">
    <div class="horizontal-container">
      <div v-if="searching.repos">Searching...</div>
      <div v-else>Enter a search term:</div>
    </div>
    <div class="horizontal-container">
      <form>
        <input
          v-model="search"
          v-on:keydown.enter.prevent
          v-on:keyup.enter="getGithubResults"
          type="text"
        />
      </form>
      <button v-on:click="getGithubResults">Search</button>
    </div>
    <div class="two-grid-container">
      <Grid
        v-if="results.length"
        v-bind:results="results"
        v-bind:showIssues="showIssues"
        v-bind:selectedRepo="selectedRepo"
        v-bind:setSelectedRepo="setSelectedRepo"
        v-bind:goToRepoPage="goToRepoPage"
        v-bind:repoPage="repoPage"
        v-bind:reposPerPage="reposPerPage"
      />
      <IssueList
        v-if="issues.length"
        v-bind:issues="issues"
        v-bind:issuePage="issuePage"
        v-bind:issuesPerPage="issuesPerPage"
        v-bind:goToIssuePage="goToIssuePage"
        v-bind:selectedRepo="selectedRepo"
        v-bind:searching="searching"
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
    selectedRepo: { id: null, open_issues: null },
    issuePage: 1,
    issuesPerPage: 40,
    reposPerPage: 20,
    repoPage: 1,
    search: "",
    searching: {
      repos: false,
      issues: false,
    },
  }),
  components: {
    IssueList,
    Grid,
  },
  methods: {
    getGithubResults: async function (page) {
      this.searching.repos = true;
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const req = await fetch(
        `https://api.github.com/search/repositories?q=${this.search}&per_page=${this.reposPerPage}&page=${page}&sort=stars&order=desc`,
        {
          headers: {
            Authorization: `token ${authToken}`,
          },
        }
      );
      const data = await req.json();
      this.results = data.items;
      this.searching.repos = false;
    },
    showIssues: async function (page) {
      this.searching.issues = true;
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const req = await fetch(
        `${this.selectedRepo.url}/issues?per_page=${this.issuesPerPage}&page=${page}`,
        {
          headers: {
            Authorization: `token ${authToken}`,
          },
        }
      );
      const data = await req.json();
      this.issues = data;
      this.searching.issues = false;
    },
    goToIssuePage: async function (newPage) {
      await this.showIssues(newPage);
      this.issuePage = newPage;
    },
    goToRepoPage: async function (newPage) {
      await this.getGithubResults(newPage);
      this.repoPage = newPage;
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
.two-grid-container {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: 1fr;
  width: 100%;
}
.two-grid-container > * {
  width: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
