<template>
  <table>
    <tr>
      <th>Name</th>
      <th>Stars</th>
    </tr>
    <tr v-for="(x, i) in results" v-bind:key="i">
      <td>{{x.name}}</td>
      <td>{{x.stargazers_count}}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: {
    results: Array,
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
}

th,
td {
  padding: 0.5rem;
}
</style>
