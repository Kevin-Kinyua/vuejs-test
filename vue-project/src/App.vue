<template>
  <div id="app">
    <Orbit />
  </div>
</template>

<script>
import Orbit from './components/Orbit.vue';

export default {
  name: 'App',
  components: {
    Orbit
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-left: -30px;
  margin-bottom: -200px;
}
</style>
