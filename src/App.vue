<template>
  <div id="app">
    <header-app/>    
    <main-content/>
  </div>
</template>

<script>
import headerApp from './components/headerApp.vue'
import mainContent from './components/mainContent.vue'
export default {
  name: 'App',
  components: {
    headerApp,
    mainContent
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
}
nav{
  margin-bottom:60px;
}
</style>
