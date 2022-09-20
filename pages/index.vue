05
<template>

  <input type="text" id="search" placeholder="Enter The Pin " />
  <input type="file" id="file" />
  <button @click="upload">Upload</button>
  <select id="layer-change">
    <option selected value="mapbox://styles/mapbox/streets-v11">Dark</option>
    <option value="mapbox://styles/mapbox/outdoors-v11">outdoors</option>
    <option value="mapbox://styles/mapbox/light-v10">light</option>
    <option value="mapbox://styles/mapbox/dark-v10">dark</option>
    <option value="mapbox://styles/mapbox/satellite-v9">satellite</option>
    <option value="mapbox://styles/mapbox/satellite-streets-v11">
      satellite-streets
    </option>
    <option value="mapbox://styles/mapbox/navigation-day-v1">
      navigation-day
    </option>
    <option value="mapbox://styles/mapbox/navigation-night-v1">
      navigation-night
    </option>
  </select>
  <main class="w-screen h-screen">
    <v-map class="w-full h-full" :options="data.options" @loaded="onMapLoaded">
    </v-map>
    </main>
</template>
<script setup lang="ts">
import { globalAgent } from "http";
import mapboxgl from "mapbox-gl";
import VMap from "v-mapbox";
function upload() {
  console.log("hii");
}
const data = reactive({
  options: {
    accessToken:
      "pk.eyJ1IjoibWF5dXJ3YWtpa2FyIiwiYSI6ImNsNmdjdGxwbjBiNGMzY282bWh0dng2c2kifQ.y-m4-zQKOeOOnDG5I1u6ng",
    style: "mapbox://styles/mapbox/satellite-streets-v11",
    container: 'map',
    center: [30,50],
    zoom: 1.5,
    maxZoom: 22,
    crossSourceCollisions: false,
    failIfMajorPerformanceCaveat: false,
    attributionControl: false,
    preserveDrawingBuffer: true,
    hash: false,
    minPitch: 0,
    maxPitch: 60,
    projection:'globe'
  } as mapboxgl.MapboxOptions,
});
async function onMapLoaded(map: mapboxgl.Map) {
  let colorSet = 0;
  let color = [
    "051E10",
    "#F92929",
    "#21F728",
    "#D4F721",
    "#F76821",
    "#F721BE",
    "#5321F7",
    "#21F77E",
    "#051E10",
  ];
  console.log(map);
  const marker = new mapboxgl.Marker({
    draggable: true,
    color: "#000000",
  });
  marker.setLngLat([75.70610046386717, 18.333587971760853]);
  marker.addTo(map);
  map.on("click", (e) => {
    colorSet++;
    console.log(e.lngLat.lat, e.lngLat.lng);
    if (colorSet === color.length) {
      colorSet = 0;
      new mapboxgl.Marker({
        draggable: true,
        color: `${color[colorSet]}`,
      })
        .setLngLat([e.lngLat.lng, e.lngLat.lat])
        .addTo(map);
    }
    new mapboxgl.Marker({
      draggable: true,
      color: `${color[colorSet]}`,
    })
      .setLngLat([e.lngLat.lng, e.lngLat.lat])
      .addTo(map);
  });
  const setStyle: any = document.getElementById("layer-change");
  setStyle.addEventListener("change", (event) => {
    console.log(event);
    map.setStyle(event.target.value);
  });
  map.flyTo({
    center: [75.70610046386717, 18.333587971760853],
    zoom: 9,
    speed: 0.9,
    curve: 1,
    easing(t) {
      return t;
    },
  });
  map.on('load', () => {
// Set the default atmosphere style
map.setFog({});
});
}
</script>
<style>
#layer-change {
  position: fixed;
  top: 10px;
  left: 10px;
  z-index: 1;
}
#file {
  width: 185px;
  position: fixed;
  top: 40px;
  right: 5px;
  z-index: 1;
}
#search {
  position: fixed;
  top: 10px;
  right: 10px;
  z-index: 1;
}
html,

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2C3E50;
}

.w-screen {
  width: 100vw;
}
.h-screen {
  height: 100vh;
}
.h-full {
  height: 100%;
}
.w-full {
  width: 100%;
}
</style>