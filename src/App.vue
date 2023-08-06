<template>
  <AppHeader />
  <!-- <MainCarousel :isAuto="true" :transition="0.8"/> -->
  <MainCarousel />
</template>
<script setup lang="ts">
import AppHeader from './components/AppHeader.vue'
import MainCarousel from './components/MainCarousel.vue'

  // import HelloWorld from './components/HelloWorld.vue'
</script>

<!-- <style lang="scss">
.wrapper {
  padding: 10px;
  // display: grid;
  // place-items: center;
  width: 300px;
  .sec {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 50px;

    textarea {
      font-family: sans-serif;
      resize: none;
      padding: 6px 12px;
      flex: 1;
    }
  }
}
</style> -->
