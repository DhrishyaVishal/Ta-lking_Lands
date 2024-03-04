<template>
  <div class="app">
    <MapContainer />
  </div>
</template>

<script>
import MapContainer from './components/MapContainer.vue';

export default {
  name: 'App',
  components: {
    MapContainer
  }
}
</script>

<style>

.app {
  text-align: center;
}
</style>