<template>
  <div v-if="issues.length === 0">
    <span>This repo does not have any logged issues</span>
    <img class="emptyIcon" src="../img/empty.svg" />
  </div>
  <div v-else>
    <div
      v-if="gfis.length > 0"
      class="gfi-tag"
      :style="{ 'background-color': getGoodFirstIssueColor() }"
    >
      good first issues: {{ gfis.length }}
    </div>
    <ul>
      <li v-for="(x, i) in issues" v-bind:key="i">
        <div class="issue">
          <span class="issue-number">{{
            i + 1 + (issuePage - 1) * issuesPerPage
          }}</span
          >: <a v-bind:href="x.html_url"> {{ x.title }}</a>
        </div>
        <div
          class="label"
          v-for="(a, b) in x.labels"
          v-bind:key="b"
          v-bind:style="getLabelStyle(a.color)"
        >
          <span class="label-text">{{ a.name }}</span>
        </div>
      </li>
    </ul>
    <Pagination
      v-bind:page="issuePage"
      v-bind:perPage="issuesPerPage"
      v-bind:entryCount="selectedRepo.open_issues"
      v-bind:goToPage="goToIssuePage"
    />
  </div>
</template>

<script>
import Pagination from "./Pagination.vue";
export default {
  name: "issue-list",
  data: () => ({
    selectedPage: null,
  }),
  components: { Pagination },
  props: {
    issues: Array,
    gfis: Array,
    issuePage: Number,
    issuesPerPage: Number,
    goToIssuePage: Function,
    searching: Object,
    selectedRepo: Object,
  },
  methods: {
    getGoodFirstIssueColor: function () {
      return (
        "#" +
        this.gfis[0].labels.filter((el) => el.name === "good first issue")[0]
          .color
      );
    },
    getLabelStyle: (color) => ({
      background: "#" + color,
      border: `1px solid ${
        color === "fff" || color === "ffffff" || color === "white"
          ? "black"
          : "none"
      }`,
    }),
    formPageTransition: function () {
      const page = parseInt(this.selectedPage);
      if (page > 0 && page * this.issuesPerPage < this.selectedRepo.open_issues)
        this.goToIssuePage(page);
      else alert("Entered page is out of bounds");
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
  margin: 5px 10px;
  align-items: center;
}
.issue {
  margin-right: 10px;
}
.label {
  width: fit-content;
  margin-right: 10px;
  padding: 5px;
}
.label-text {
  background: inherit;
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  filter: invert(1) grayscale(1) contrast(10);
}
.btn-container {
  display: flex;
  align-items: center;
}
.gfi-tag {
  padding: 5px;
}
</style>
