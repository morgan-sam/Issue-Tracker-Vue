<template>
  <div>
    <table>
      <tr>
        <th>Index</th>
        <th>Name</th>
        <th>Stars</th>
        <th>Issues</th>
      </tr>
      <tr
        v-for="(x, i) in results"
        v-bind:key="i"
        v-bind:class="[x.id === selectedRepo.id ? 'selected-repo' : '']"
      >
        <td>{{ i + 1 }}</td>
        <td>
          <span class="name-text" v-on:click="setSelectedRepo(x)">{{
            x.name
          }}</span>
        </td>
        <td>{{ x.stargazers_count }}</td>
        <td>{{ x.open_issues }}</td>
      </tr>
    </table>
    <div class="btn-container">
      <button v-on:click="goToRepoPage(repoPage - 1)">Previous</button>
      <button v-on:click="goToRepoPage(repoPage + 1)">Next</button>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    repoPage: 1,
  }),
  props: {
    results: Array,
    showIssues: Function,
    selectedRepo: Object,
    setSelectedRepo: Function,
  },
  methods: {
    goToRepoPage: function (page) {
      console.log(page);
    },
  },
};
</script>

<style scoped>
table {
  display: table;
  margin: 1rem;
}
table,
th,
td {
  border: 1px solid black;
  user-select: none;
}

th,
td {
  padding: 0.5rem;
}

.name-text {
  cursor: pointer;
}
.name-text:hover {
  color: red;
}
.selected-repo {
  background-color: salmon;
  color: white;
}
.btn-container {
  display: flex;
  align-items: center;
}
</style>
