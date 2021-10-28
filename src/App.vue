<template>
  <div id="app">
    <SortingSystem msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import SortingSystem from './components/SortingSystem.vue'
import './style.scss';

export default {
  name: 'App',
  components: {
    SortingSystem
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
