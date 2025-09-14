<template>
  <div id="app">
   <home-page/>
  </div>
</template>

<script>
import HomePage from './components/Home/HomePage.vue';

//import LoginPage from "./components/Login/LoginPage.vue"
export default {
  name: 'App',
  components: {
     HomePage
  }
}
</script>

<style>

</style>
