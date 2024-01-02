<template>
  <div id="app">
    <list-service></list-service>>
  </div>
</template>

<script>
import ListService from './components/ListService.vue'

export default {
  name: 'App',
  components: {
    
    ListService
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
