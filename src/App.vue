<template>
  <div id="app">
    <h1>{{ isGermanyView ? "Germany" : "🌍 World Map" }}</h1>

    <button class="toggle-button" @click="toggleView">
      {{ isGermanyView ? "World View" : "Germany View" }}
    </button>

    <l-map
  ref="mapRef"
  style="height: 700px; width: 100%; border-radius: 12px; overflow: hidden;"
  :zoom="zoom"
  :center="center"
  :options="mapOptions"
  @zoom="onZoom"
  @zoomend="onZoomEnd"
>
  <l-geo-json :geojson="world" :options-style="geojsonOptions" :key="isGermanyView" />
</l-map>
  </div>
</template>

<script setup>
import { ref } from "vue"
import { LGeoJson, LMap } from "@vue-leaflet/vue-leaflet"
import world from "./assets/world.geo.json"
import germany from "./assets/germany.geo.json"

const mapOptions = {
  zoomControl: false,
  scrollWheelZoom: false,
  dragging: false,
  doubleClickZoom: false,
  touchZoom: false,
  keyboard: false,
  boxZoom: false,
  attributionControl: false,
  zoomAnimation: false,
  minZoom: 1,
  maxZoom: 8
}

const mapRef = ref(null)
const isGermanyView = ref(false)
const zoom = ref(2)
const center = ref([45, 0])

const geojsonOptions = (feature) => {
  const isGermany = feature?.properties?.name === "Germany"
  if (isGermanyView.value) {
    return {
      fillColor: isGermany ? "#C4D624" : "#C4D624",
      color: isGermany ? "#C4D624" : "#C4D624",
      weight: 1,
      fillOpacity: isGermany ? 1 : 0.5,
    }
  } else {
    return {
      fillColor: "#C4D624",
      weight: 1,
      color: "#C4D624",
      fillOpacity: 1,
    }
  }
}

function onZoom() {
  const map = mapRef.value?.leafletObject
  if (!map) return

  map.eachLayer(layer => {
    if (layer.setStyle) layer.setStyle(geojsonOptions(layer.feature))
  })
}

function onZoomEnd() {
  const map = mapRef.value?.leafletObject
  if (!map) return

  map.invalidateSize()
}

function toggleView() {
  const map = mapRef.value?.leafletObject
  if (!map) return

  if (!isGermanyView.value) {

    map.flyTo([51, 10], 6, { duration: 1, easeLinearity: 0.25 })

    map.once("zoomend", () => {
      isGermanyView.value = !isGermanyView.value
      map.invalidateSize()
      map.eachLayer(layer => {
        if (layer.setStyle) layer.setStyle(geojsonOptions(layer.feature))
      })
    })
  } else {
    map.flyTo([45, 0], 2, { duration: 1, easeLinearity: 0.25 })

    map.once("zoomend", () => {
      isGermanyView.value = false
      map.invalidateSize()
    })
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

.leaflet-interactive {
  transition: fill 1s ease, stroke 1s ease;
}
</style>