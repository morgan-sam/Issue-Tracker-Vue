<template>
  <table>
    <tr>
      <th>Name</th>
      <th>Stars</th>
      <th>Issues</th>
    </tr>
    <tr v-for="(x, i) in results" v-bind:key="i">
      <td class="name-cell" v-on:click="cellClicked(x.name)">{{x.name}}</td>
      <td>{{x.stargazers_count}}</td>
      <td>{{x.open_issues}}</td>
    </tr>
  </table>
</template>

<script>
export default {
  props: {
    results: Array,
  },
  methods: {
    cellClicked: function (name) {
      console.log(name);
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

.name-cell {
  cursor: pointer;
}
</style>
