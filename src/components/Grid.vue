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
        v-for="(x, i) in repos"
        v-bind:key="i"
        v-bind:class="[x.id === selectedRepo.id ? 'selected-repo' : '']"
      >
        <td>{{ i + 1 + (repoPage - 1) * reposPerPage }}</td>
        <td>
          <span class="name-text" v-on:click="setSelectedRepo(x)">
            {{
            x.name
            }}
          </span>
        </td>
        <td>{{ x.stargazers_count }}</td>
        <td>{{ x.open_issues }}</td>
      </tr>
    </table>
    <Pagination
      v-bind:page="repoPage"
      v-bind:perPage="reposPerPage"
      v-bind:entryCount="reposCount"
      v-bind:maxEntryCount="maxReposCount"
      v-bind:goToPage="goToRepoPage"
    />
  </div>
</template>

<script>
import Pagination from "./Pagination.vue";
export default {
  data: () => ({
    maxReposCount: 1000,
  }),
  props: {
    repos: Array,
    reposCount: Number,
    showIssues: Function,
    selectedRepo: Object,
    setSelectedRepo: Function,
    goToRepoPage: Function,
    repoPage: Number,
    reposPerPage: Number,
  },
  components: { Pagination },
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
