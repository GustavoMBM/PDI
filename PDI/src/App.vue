<script setup>
import { RouterLink, RouterView } from 'vue-router'
import NavBar from './components/NavBar.vue';
import Dashboard from './components/Dashboard.vue';

</script>

<template >
  <div class="min-h-screen bg-gradient-to-br from-[#0A0A0B] via-[#0D121A] to-[#071726]
text-white flex flex-col items-center">

    <NavBar />
    <Dashboard />
  </div>
</template>

<style scoped>
</style>
