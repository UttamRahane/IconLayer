<template>
  <div id="app">
    <map-component/>
  </div>
</template>

<script>
import MapComponent from "./components/MapComponent";
export default {
  name: "App",
  components: {
    MapComponent
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
