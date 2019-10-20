<template>
  <div class="map-wrapper">
    <div id="map" class="map"></div>
    <Modal
      v-if="modalOpened && selectedMarker"
      v-on:modalClosed="modalOpened = false"
      v-bind:data="selectedMarker"
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
      selectedMarker: null,
      redIcon: new L.Icon({
        iconUrl:
          "https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png",
        shadowUrl:
          "https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png",
        iconSize: [25, 41],
        iconAnchor: [12, 41],
        popupAnchor: [1, -34],
        shadowSize: [41, 41]
      })
    };
  },
  methods: {
    fetchData() {
      const axios = require("axios");
      axios.get("http://127.0.0.1:8000/api/hackaton/").then(response => {
        // handle success
        this.data = response.data;
        this.insertMarkers();
      });
    },
    insertMarkers() {
      this.data.forEach(house => {
        let marker = this.leafet
          .marker([house.lat, house.lng], { icon: this.redIcon })
          .on("click", e => {
            console.log(e);
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
