<script setup>
import { RouterLink, RouterView } from 'vue-router'
import Navbar from './components/Navbar.vue';
import HelloWorld from './components/Navbar.vue'


</script>

<template>
  <Navbar />
  <RouterView />
  
  <div>
    <SearchBar />
  </div>
</template>

<style scoped>

</style>
