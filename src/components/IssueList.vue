<template>
  <div v-if="searching.issues">Loading...</div>
  <div v-else>
    <ul>
      <li v-for="(x, i) in issues" v-bind:key="i">
        <div class="issue">
          {{ i + 1 + (issuePage - 1) * issuesPerPage }}: {{ x.title }}
        </div>
        <div
          class="label"
          v-for="(a, b) in x.labels"
          v-bind:key="b"
          v-bind:style="getLabelStyle(a.color)"
        >
          {{ a.name }}
        </div>
      </li>
    </ul>
    <div class="btn-container">
      <button v-if="this.issuePage > 1" v-on:click="changeIssuePage(-1)">
        Previous
      </button>
      <div>{{ getIssuesSpanText() }}</div>
      <button
        v-if="
          this.issuePage * this.issuesPerPage < this.selectedRepo.open_issues
        "
        v-on:click="changeIssuePage(1)"
      >
        Next
      </button>
    </div>
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
    searching: Object,
    selectedRepo: Object,
  },
  methods: {
    getLabelStyle: (color) => ({
      background: "#" + color,
      border: `1px solid ${
        color === "fff" || color === "ffffff" || color === "white"
          ? "black"
          : "none"
      }`,
    }),
    getIssuesSpanText: function () {
      const min = 1 + (this.issuePage - 1) * this.issuesPerPage;
      const max = Math.min(
        this.issuePage * this.issuesPerPage,
        this.selectedRepo.open_issues
      );
      return `Issues ${min} to ${max} of ${this.selectedRepo.open_issues}`;
    },
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
.btn-container {
  display: flex;
  align-items: center;
}
</style>
