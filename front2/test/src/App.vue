<template>
  <div id="app">
    <main_page></main_page>
  </div>
</template>

<script>
  // import HelloWorld from './components/HelloWorld.vue'
  
  

  import main_page from "./components/main_page.vue"
  
  export default {
    name: 'App',
    
    // mounted () {
    //   axios.get('http://localhost:8000/quickstart/users/').then((response) => {
    //     this.notes = response.data
    //   })
    // }
    components:{
      main_page
    }
  }

</script>