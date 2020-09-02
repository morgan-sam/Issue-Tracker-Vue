<template>
  <table>
    <tr>
      <th>Name</th>
      <th>Stars</th>
      <th>Issues</th>
    </tr>
    <tr v-for="(x, i) in results" v-bind:key="i">
      <td>
        <span class="name-text" v-on:click="listIssues(x)">{{ x.name }}</span>
      </td>
      <td>{{ x.stargazers_count }}</td>
      <td>{{ x.open_issues }}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: {
    results: Array,
  },
  methods: {
    listIssues: async function (repo) {
      const authToken = process.env.VUE_APP_GITHUB_AUTH_TOKEN;
      const req = await fetch(`${repo.url}/issues`, {
        headers: {
          Authorization: `token ${authToken}`,
        },
      });
      const data = await req.json();
      console.log(data);
      return null;
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
</style>
