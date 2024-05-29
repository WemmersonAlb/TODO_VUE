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
::-webkit-scrollbar {
  width: 7px;
}

::-webkit-scrollbar-track {
  border-radius: 10px;
  background: #222; 
}

::-webkit-scrollbar-thumb {
  background: #D9D9D9;
  border-radius: 10px;
}
</style>