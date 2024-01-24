<template>
  <ImageBase64 />
</template>

<script>
import ImageBase64 from './components/ImageBase64.vue';

export default {
  name: 'App',
  components: {
    ImageBase64
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
