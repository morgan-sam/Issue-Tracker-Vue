<template>
  <ul>
    <li>Issue 1</li>
    <li>Issue 2</li>
    <li>Issue 3</li>
    <li>Issue 4</li>
    <li>Issue 5</li>
  </ul>
</template>

<script>
export default {
  name: 'issue-list',
  props: {
    msg: String
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
