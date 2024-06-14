<template>
  <div id="mapDiv">
    <l-map ref="map" v-model:zoom="zoom" :center="[lat, lon]">
      <l-tile-layer
          url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
          layer-type="base"
          name="OpenStreetMap"
      ></l-tile-layer>
      <l-marker :lat-lng="[lat,lon]" ></l-marker>
    </l-map>
  </div>
</template>

<script>
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer, LMarker } from "@vue-leaflet/vue-leaflet";

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker
  },
  data() {
    return {
      zoom: 11,
    };
  },
  props: {
    lat: {
      type: Number,
      required: true,
    },
    lon: {
      type: Number,
      required: true,
    },
  },
};
</script>

<style>
  #mapDiv {
    height: 300px;
    width: 300px;
  }
  @media (max-width: 768px) {
    #mapDiv{
      height:200px;
      width:200px;
    }
  }
</style>