<template>
  <div>
    
    <!-- <xlsx-test /> -->
    <map-mapping />
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
// import HelloWorld from './components/HelloWorld.vue';
// import XlsxTest from './components/XlsxTest.vue'
import MapMapping from './components/MapMapping/index.vue';

export default defineComponent({
  name: 'App',
  components: {
    // HelloWorld,
    // XlsxTest,
    MapMapping
  }
});
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
