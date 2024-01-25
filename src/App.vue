<script setup>
import { RouterLink, RouterView } from 'vue-router';
import UtcIcon from './components/icons/IconUniversity.vue';

</script>

<template>
  <header>
    <UtcIcon />
  </header>
  <main>
    <RouterView />
  </main>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
</style>
