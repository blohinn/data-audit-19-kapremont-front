<template>
  <div class="map-view">
    <Map></Map>
  </div>
</template>

<script>
import Map from "@/components/Map.vue";

export default {
  name: "MapView",
  components: {
    Map
  }
};
</script>

<style scoped>
.map-view {
  height: 100%;
}
</style>
