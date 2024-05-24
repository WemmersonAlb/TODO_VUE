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
  <v-app class="app">
    <div class="main-container">
      <Navbar />
      <RouterView />
    </div>
  </v-app>
</template>
<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');
.app{
  background-color: #222;
}
</style>