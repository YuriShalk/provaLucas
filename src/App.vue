<template>

  <TheHeader />

  <img alt="App logo" src="./assets/logo.jpg">

  <ListaTarefas />

</template>

<script>
import TheHeader from './components/TheHeader.vue'
import ListaTarefas from './components/ListaTarefas.vue'

export default {
  name: 'App',
  components: {
    TheHeader,
    ListaTarefas
    
  },
  data() {
    return {
      
    }
  },
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
