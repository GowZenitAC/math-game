<script setup>
  import ButtonStart from '@/components/ui/ButtonStart.vue';
  import Title from '@/components/Title.vue';
  
</script>

<template>
  <main>
    <div class="container">
      <Title/>
      <ButtonStart/>
    </div>
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