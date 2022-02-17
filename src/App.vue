<template>
  <HeaderComponent />
  <ApiComponent/>
</template>

<script>
import ApiComponent from './components/ApiComponent.vue'
import HeaderComponent from './components/HeaderComponent.vue'

export default {
  name: 'App',
  components: {
    HeaderComponent,
    ApiComponent
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  background: #E5E3C9;
  /*margin-top: 60px;*/
}
body {
  font-size: 16px;
}
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
</style>
