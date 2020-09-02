<template>
  <div>
    <ul>
      <li v-for="(x, i) in issues" v-bind:key="i">
        {{ i + 1 }}: {{ x.title }}
      </li>
    </ul>
    <button v-on:click="previousPage()">Previous</button>
    <button v-on:click="nextPage()">Next</button>
  </div>
</template>

<script>
export default {
  name: "issue-list",
  props: {
    issues: Array,
  },
  methods: {
    previousPage: function () {
      console.log("Previous page");
    },
    nextPage: function () {
      console.log("Next page");
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
  text-align: left;
  display: inline-block;
  margin: 0 10px;
}
</style>
