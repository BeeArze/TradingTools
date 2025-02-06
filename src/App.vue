<template>
  <div id="app">
    <Home />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Home from './views/Home.vue';

export default defineComponent({
  name: 'App',
  components: {
    Home
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  height: 100vh;
  display: flex;
  flex-direction: column;
}
body {
  margin: 0;
  padding: 0;
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
}
</style>
