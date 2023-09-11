<script setup>
import TheWelcome from './components/TheWelcome.vue'
</script>

<template>
  <main>
    <TheWelcome />
  </main>
</template>

<style lang="scss">
  $primary-500: #29915d; // hover
  $primary-600: #1c7247; // default
  //$primary-700: #462339; // hover-light-theme
  $grey-hue: 260;
  $grey-600: hsl($grey-hue, 35%, 41%);
  $grey-900: hsl($grey-hue, 35%, 26%);
  
  // import Pico
  @import "@picocss/pico/scss/pico";
  
</style>
