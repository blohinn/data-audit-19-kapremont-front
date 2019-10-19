<template>
  <div id="map" class="map"></div>
</template>

<script>
export default {
  name: "Map",
  created() {
    // eslint-disable-next-line no-undef
    this.leafet = L;
  },
  mounted() {
    this.map = this.leafet
      .map("map", {
        preferCanvas: true,
        minZoom: 6,
        zoomControl: false
      })
      .setView([54.318463, 48.397007], 13); // center, zoom

    this.leafet
      .tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png")
      .addTo(this.map);
  }
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
}
</style>
