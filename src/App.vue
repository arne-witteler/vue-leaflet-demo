<template>
  <div id="app">
    <h1>🌍 Weltkarte (statisch)</h1>

    <l-map
      style="height: 700px; width: 100%; border-radius: 12px; overflow: hidden;"
      :zoom="2"
      :center="[45, 0]"
      :options="mapOptions"
    >
      <l-tile-layer
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        attribution=""
      />
    </l-map>
  </div>
</template>

<script setup>
import L from "leaflet"
import { LMap, LTileLayer } from "@vue-leaflet/vue-leaflet"

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
</style>