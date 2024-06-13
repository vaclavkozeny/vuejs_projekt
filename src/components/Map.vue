<template>
  <div id="map" :style="{ height: '500px', width: '100%' }"></div>
</template>

<script>
import { defineComponent, onMounted, watch } from 'vue';
import L from 'leaflet';
import 'leaflet/dist/leaflet.css';

export default defineComponent({
  name: 'Map',
  props: {
    latitude: {
      type: Number,
      required: true
    },
    longitude: {
      type: Number,
      required: true
    }
  },
  setup(props) {
    let myMap;

    onMounted(() => {
      myMap = L.map('map').setView([props.latitude, props.longitude], 13);

      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(myMap);

      L.marker([props.latitude, props.longitude]).addTo(myMap)
          .bindPopup('A pretty CSS3 popup.<br> Easily customizable.')
          .openPopup();
    });

    watch(() => [props.latitude, props.longitude], ([newLat, newLng]) => {
      if (myMap) {
        myMap.setView([newLat, newLng], 13);
      }
    });

    return {};
  }
});
</script>

<style scoped>
#map {
  height: 100%;
  width: 100%;
}
</style>