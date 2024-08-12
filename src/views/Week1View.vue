<script setup>
  import Week1Item from '../components/Week1Item.vue'
</script>

<template>
  <div class="about">
    <Week1Item />
  </div>
</template>

<style>
  @media (min-width: 1024px) {
    .about {
      min-height: 100vh;
      display: flex;
      align-items: center;
    }
  }
</style>
