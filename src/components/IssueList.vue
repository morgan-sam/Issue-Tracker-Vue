<template>
  <ul>
      <li v-for="x in issues" v-bind:key="x">
        {{x.name}}
      </li>
  </ul>
</template>

<script>
export default {
  name: 'issue-list',
  props: {
    issues: Array
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  display: flex;
  flex-direction: column;
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
</style>
