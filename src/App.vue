<script setup>
import CryptoPrice from './components/solana/CryptoPrice.vue'
</script>

<template>
  <RouterView />
  <CryptoPrice />
</template>

<style lang="scss">
#app {
  font-family: 'Inter', Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  margin: 0;
  min-width: 20rem;
}
</style>
