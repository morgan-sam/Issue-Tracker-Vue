<template>
  <div>
    <ul>
      <li v-for="(x, i) in issues" v-bind:key="i">
        {{ i + 1 + (issuePage - 1) * 30 }}: {{ x.title }}
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
  text-align: left;
  display: inline-block;
  margin: 0 10px;
}
</style>
