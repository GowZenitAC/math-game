<script setup>
  import ButtonStart from '@/components/ui/ButtonStart.vue';
  import Title from '@/components/Title.vue';
  import logo from '@/components/ui/logo.vue';
</script>

<template>
  <logo/>
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
  position: relative;
  top: -120px;
}
</style>