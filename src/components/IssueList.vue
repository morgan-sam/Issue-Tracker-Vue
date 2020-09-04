<template>
  <div>
    <ul>
      <li v-for="(x, i) in issues" v-bind:key="i">
        <div class="issue">
          {{ i + 1 + (issuePage - 1) * issuesPerPage }}: {{ x.title }}
        </div>
        <div
          class="label"
          v-for="(a, b) in x.labels"
          v-bind:key="b"
          v-bind:style="{ background: '#' + a.color }"
        >
          {{ a.name }}
        </div>
      </li>
    </ul>
    <button v-on:click="changeIssuePage(-1)">Previous</button>
    <button v-on:click="changeIssuePage(1)">Next</button>
  </div>
</template>

<script>
export default {
  name: "issue-list",
  props: {
    issues: Array,
    issuePage: Number,
    issuesPerPage: Number,
    changeIssuePage: Function,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  display: flex;
  flex-direction: column;
  list-style-type: none;
  width: fit-content;
  padding: 1rem;
}
li {
  display: flex;
  text-align: left;
  margin: 0 10px;
}
.issue {
  margin-right: 10px;
}
.label {
  width: fit-content;
  margin-right: 10px;
}
</style>
