<template>
  <div id="app">
    <encryption-raffle/>
  </div>
</template>

<script>
import EncryptionRaffle from './components/EncryptionRaffle.vue'

export default {
  name: 'App',
  components: {
    EncryptionRaffle
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
