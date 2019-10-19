<template>
  <div id="map" class="map"></div>
</template>

<script>
export default {
  name: "Map",
  data() {
    return {
      leaflet: null,
      map: null,
      markersLayerGroup: null,
      data: [],
      markers: []
    };
  },
  methods: {
    fetchData() {
      this.data = [
        {
          lat: 55.796289,
          lng: 49.108795,
          meta: 1111
        }
      ];
    },
    insertMarkers() {
      this.data.forEach(house => {
        let marker = this.leafet
          .marker([house.lat, house.lng])
          .on("click", e => console.log(e));
        marker.meta = house;
        marker.addTo(this.markersLayerGroup);
      });
    }
  },
  beforeCreate() {
    // eslint-disable-next-line no-undef
    this.leafet = L;
  },
  created() {
    this.fetchData();
  },
  mounted() {
    this.map = this.leafet
      .map("map", {
        preferCanvas: true,
        minZoom: 6,
        zoomControl: false
      })
      .setView([55.796289, 49.108795], 12); // center, zoom

    this.leafet
      .tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png")
      .addTo(this.map);

    this.markersLayerGroup = this.leafet.layerGroup().addTo(this.map);

    this.insertMarkers();
  }
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
}
</style>
