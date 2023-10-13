<template>
  <div>
  <HomePage />
  </div>
</template>

<script>
import HomePage from './components/Home/HomeContent.vue'

export default {
  name: 'App',
  components: {
    HomePage,
    
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
body{
 background:linear-gradient(16deg,#00feba,#4b548a);
 background-repeat: no-repeat;
 background-attachment: fixed;
}
</style>
