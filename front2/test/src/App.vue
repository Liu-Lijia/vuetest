<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  {% raw %}{{ notes }}
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import axios from 'axios'

export default {
  name: 'App',
  data: () => ({
    notes: []
  }),
  components: {
    HelloWorld
  },
  mounted () {
  axios.get('http://localhost:8000/api/notes/').then((response) => {
    this.notes = response.data
  })
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
