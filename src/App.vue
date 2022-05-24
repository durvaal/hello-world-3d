<template>
  <HelloWorld />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  mounted() {
    document.title = "Hello World 3D";
  }
}
</script>

<style>
body {
  margin: 0px;
  padding: 0px;
  width: 100%;
  height: 100%;
}
</style>
