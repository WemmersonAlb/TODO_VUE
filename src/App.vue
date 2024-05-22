<script>
import { RouterLink, RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue'

export default{
  components:{
    Navbar
  }
}
</script>

<template>
  <div class="main-container">
    <Navbar />
    <RouterView />
  </div>
</template>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
</style>