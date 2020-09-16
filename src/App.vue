<template>
  <div class="app">
    <div class="header">
      <h1 class="app-title">GitHub Issue Tracker</h1>
      <div class="horizontal-container">
        <div>Enter a search term:</div>
      </div>
      <div class="horizontal-container">
        <form>
          <input
            v-model="search"
            v-on:keydown.enter.prevent
            v-on:keyup.enter="showRepos(1)"
            type="text"
          />
        </form>
        <button v-on:click="showRepos">Search</button>
      </div>
    </div>
    <div class="two-grid-container">
      <Grid
        v-if="repos ? repos.length > 0 && !searching.repos : false"
        v-bind:repos="repos"
        v-bind:reposCount="reposCount"
        v-bind:showIssues="showIssues"
        v-bind:selectedRepo="selectedRepo"
        v-bind:setSelectedRepo="setSelectedRepo"
        v-bind:goToRepoPage="goToRepoPage"
        v-bind:repoPage="repoPage"
        v-bind:reposPerPage="reposPerPage"
      />
      <div v-if="searching.repos">Searching Repos...</div>
      <IssueList
        v-if="issues ? issues.length > 0 && !searching.issues : false"
        v-bind:issues="issues"
        v-bind:issuePage="issuePage"
        v-bind:issuesPerPage="issuesPerPage"
        v-bind:goToIssuePage="goToIssuePage"
        v-bind:selectedRepo="selectedRepo"
        v-bind:searching="searching"
      />
      <div v-if="searching.issues">Searching Issues...</div>
    </div>
  </div>
</template>

<script>
import IssueList from "./components/IssueList.vue";
import Grid from "./components/Grid.vue";

export default {
  name: "App",
  data: () => ({
    repos: [],
    reposCount: 0,
    repoPage: 1,
    reposPerPage: 15,

    issues: [],
    issuePage: 1,
    issuesPerPage: 20,

    selectedRepo: { id: null, open_issues: null },

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
    showRepos: async function (page) {
      console.log(page);
      this.searching.repos = true;
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const url = `https://api.github.com/search/repositories?q=${this.search}&per_page=${this.reposPerPage}&page=${page}&sort=stars&order=desc`;
      const req = await fetch(url, {
        headers: { Authorization: `token ${authToken}` },
      });
      const data = await req.json();
      this.repos = data.items;
      this.reposCount = data.total_count;
      this.searching.repos = false;
    },
    showIssues: async function (page) {
      this.searching.issues = true;
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const url = `${this.selectedRepo.url}/issues?per_page=${this.issuesPerPage}&page=${page}`;
      const req = await fetch(url, {
        headers: { Authorization: `token ${authToken}` },
      });
      const data = await req.json();
      this.issues = data;
      this.searching.issues = false;
    },
    goToIssuePage: async function (newPage) {
      await this.showIssues(newPage);
      this.issuePage = newPage;
    },
    goToRepoPage: async function (newPage) {
      await this.showRepos(newPage);
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
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  box-sizing: border-box;
  min-height: 100vh;
}
.app-title {
  font-size: 2rem;
  margin: 2rem;
}
button {
  width: fit-content;
  margin: 0.5rem;
}
.horizontal-container {
  margin: 0.5rem;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
}
.header {
  flex: 0 1 auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin: 2rem;
}
.two-grid-container {
  display: grid;
  flex: 1 1 auto;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: 1fr;
}
.two-grid-container > * {
  width: 50vw;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
