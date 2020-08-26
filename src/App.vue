<template>
  <div id="app">
    <IssueList v-bind:issues='[1,2,3,4,5]'/>
  </div>
</template>

<script>
import IssueList from './components/IssueList.vue'

export default {
  name: 'App',
  components: {
    IssueList,
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
