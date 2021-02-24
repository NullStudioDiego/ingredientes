<template>
  <div id="app" class="container-fluid" style="background-color: #DADED4;">
    <!--<img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <app-lista/>
  </div>
</template>

<script>
//import HelloWorld from './components/HelloWorld.vue'
import Lista from './components/Lista'

export default {
  name: 'App',
  components: {
    appLista: Lista
  }
}
</script>

<style scoped>
  #app {
    background-color: #DADED4;
    min-height: 100vh;
    height: 100%;
    width: 100%;
  }
</style>
