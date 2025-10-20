<template>
  <div id="app">
    <h1>🌍 Weltkarte (statisch)</h1>

    <l-map
  style="height: 700px; width: 100%; border-radius: 12px; overflow: hidden;"
  :zoom="2"
  :center="[45, 0]"
  :options="mapOptions"
>
  <l-geo-json :geojson="geojson" :options-style="geojsonOptions" />
</l-map>
  </div>
</template>

<script setup>
import { ref } from "vue"
import L from "leaflet"
import { LGeoJson, LMap, LTileLayer } from "@vue-leaflet/vue-leaflet"
import geojson from "./assets/world.geo.json"

const mapOptions = {
  zoomControl: false,
  scrollWheelZoom: false,
  dragging: false,
  doubleClickZoom: false,
  touchZoom: false,
  keyboard: false,
  boxZoom: false,
  attributionControl: false,
}

const geojsonOptions = () => ({
    fillColor: "#C4D624",
    weight: 1,
    color: "#C4D624",
    fillOpacity: 1,
})
</script>

<style>
body {
  margin: 0;
  font-family: system-ui, sans-serif;
  background: #fff;
  display: flex;
  justify-content: center;
}
#app {
  width: 1000px;
  margin-top: 60px;
  text-align: center;
}

.leaflet-container {
  background: #ffffff !important;
}
</style>