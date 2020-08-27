<template>
  <ul>
    <li v-for="(x, i) in issues" v-bind:key="i">{{ i }}: {{ x.name }}</li>
  </ul>
</template>

<script>
export default {
  name: "issue-list",
  props: {
    issues: Array,
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
