<template>
  <div id="app">
    <!-- <cus-manager></cus-manager>> -->
    <cusmanager-1></cusmanager-1>
    <!-- <cus-employee></cus-employee> -->
  </div>
</template>

<script>
import CusEmployee from "./components/CusEmployee.vue";
import CusManager from "./components/CusManager.vue";
import Cusmanager1 from './components/Cusmanager1.vue';

export default {
  name: "App",
  components: {
    CusEmployee,
    Cusmanager1,
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
