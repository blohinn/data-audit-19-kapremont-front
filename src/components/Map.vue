<template>
  <div class="map-wrapper">
    <div id="map" class="map"></div>
    <Modal
      v-if="modalOpened && selectedMarker"
      v-on:modalClosed="modalOpened = false"
      v-bind:pk="selectedMarker.meta"
    ></Modal>
  </div>
</template>

<script>
import Modal from "@/components/Modal";

export default {
  name: "Map",
  components: { Modal },
  data() {
    return {
      leaflet: null,
      map: null,
      markersLayerGroup: null,
      data: [],
      markers: [],
      modalOpened: true,
      selectedMarker: null
    };
  },
  methods: {
    fetchData() {
      this.data = [
        {
          lat: 55.796289,
          lng: 49.108795,
          meta: 1111
        },
        {
          lat: 55.796085,
          lng: 49.10809,
          meta: 2222
        }
      ];
    },
    insertMarkers() {
      this.data.forEach(house => {
        let marker = this.leafet
          .marker([house.lat, house.lng])
          .on("click", e => {
            this.selectedMarker = e.target.meta;
            this.modalOpened = true;
          });
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
.map-wrapper {
  height: 100%;
  width: 100%;
}

.map {
  height: 100%;
  width: 100%;
}
</style>
