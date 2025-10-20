<template>
  <div id="app">
    <h1>🌍 Weltkarte (statisch)</h1>

    <button class="toggle-button" @click="toggleView">
      {{ isGermanyView ? "Weltansicht" : "Deutschlandansicht" }}
    </button>

    <l-map
  ref="mapRef"
  style="height: 700px; width: 100%; border-radius: 12px; overflow: hidden;"
  :zoom="zoom"
  :center="center"
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

const mapRef = ref(null)
const isGermanyView = ref(false)
const zoom = ref(2)
const center = ref([45, 0])

const geojsonOptions = () => ({
    fillColor: "#C4D624",
    weight: 1,
    color: "#C4D624",
    fillOpacity: 1,
})

function toggleView() {
  const map = mapRef.value?.leafletObject
  isGermanyView.value = !isGermanyView.value

  if (map) {
    if (isGermanyView.value) {
      map.flyTo([51, 10], 6, { duration: 1 })
    } else {
      map.flyTo([45, 0], 2, { duration: 1 })
    }
  }
}
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

.toggle-button {
  margin-bottom: 20px;
  background-color: #c4d624;
  border: none;
  color: #000;
  padding: 10px 20px;
  border-radius: 10px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.2s;
}
.toggle-button:hover {
  background-color: #d8e85a;
}
</style>