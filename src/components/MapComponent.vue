<script setup lang="ts">
import * as L from 'leaflet'

import WaypointMenu from './WaypointMenu.vue'
import { computed, onMounted, ref } from 'vue'

interface Markers {
  name: string
  latitude: number
  longitude: number
}

let drawnMarkers: L.Marker[] = []
let waypointCount: number = 0
let index: number = -1 // minus 1 because 0 is a valid index
let latitude = ref(58.2781985)
let longitude = ref(26.8827179)
let latitudeWay = ref(58.2781985)
let longitudeWay = ref(26.8827179)
let waypoints = ref<Markers[]>([])

let clickTimer: any = null
let engineIsOn = ref(false)
let wayListIsActive = ref(false)
let openWaypointMenu = ref(false)
let map: L.Map | null = null

let rover: any

const engineText = computed(() => {
  return engineIsOn.value ? 'Stop Engine' : 'Start Engine'
})

const wayMenuText = computed(() => {
  return wayListIsActive.value ? 'Hide Waypoints' : 'Show Waypoints'
})

let roverIcon = L.icon({
  iconUrl: 'public/favicon_rove.png',
  iconSize: [32, 32],
  iconAnchor: [16, 16]
})

function addWaypoint(latitude: number, longitude: number) {
  let waypointName: string = 'Waypoint ' + waypointCount
  waypointCount++
  waypoints.value.push({ name: waypointName, latitude: latitude, longitude: longitude })
}

function driveToWaypoint(latitudeNew: number, longitudeNew: number) {
  rover.setLatLng([latitudeNew, longitudeNew])
  latitude.value = latitudeNew
  longitude.value = longitudeNew
}

function renameWaypoint(index: number, newName: string) {
  if (index !== null && index >= 0 && index < waypoints.value.length) {
    waypoints.value[index].name = newName
  }
}
function removeWaypoint(index: number) {
  waypoints.value.splice(index, 1)
  removeWaypointFromMap(waypoints[index].name)
}

function removeWaypointFromMap(waypointName: string) {
  drawnMarkers.forEach((marker) => {
    if (marker.getPopup()?.getContent() == waypointName) {
      marker.remove()
    }
  })
  waypoints.value.forEach((waypoint, index) => {
    if (waypoint.name === waypointName) {
      waypoints.value.splice(index, 1)
    }
  })
}

function handleEngine() {
  engineIsOn.value = !engineIsOn.value
}

function handleWayMenu() {
  wayListIsActive.value = !wayListIsActive.value
}
function activateMenu() {
  openWaypointMenu.value = true
}
function deactivateMenu() {
  openWaypointMenu.value = false
}

const handleSelection = (selectionValue: number) => {
  switch (selectionValue) {
    case 1:
      deactivateMenu();
      driveToWaypoint(latitudeWay.value, longitudeWay.value)
      removeWaypoint(index)
      break
    case 2:
      deactivateMenu()
      addWaypoint(latitudeWay.value, longitudeWay.value)
      break
    case 3:
      deactivateMenu();
      removeWaypoint(index)
      break
  }
}

onMounted(() => {
  map = L.map('map').setView([latitude.value, longitude.value], 15)
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution:
      '&copy; <a href="https://www.openstreetmap.org/copyright">RoverMap</a> start the engine and use A,W,S,D to move the rover'
  }).addTo(map)

  rover = L.marker([latitude.value, longitude.value], { icon: roverIcon }).addTo(map) // adds the rover to the map

  map.on('mousedown', (e) => {
    clickTimer = setTimeout(() => {
      const { lat, lng } = e.latlng
      latitudeWay.value = lat
      longitudeWay.value = lng
      addWaypoint(latitudeWay.value, longitudeWay.value)
      activateMenu()
      drawWaypoints()
    }, 500)
  })

  function drawWaypoints() {
    drawnMarkers.forEach((marker) => {
      if (map !== null) {
        map.removeLayer(marker)
      }
    })
    drawnMarkers = []
    waypoints.value.forEach((waypoint) => {
      if (map !== null) {
        const waypointTwo = L.marker([waypoint.latitude, waypoint.longitude])
          .bindPopup(waypoint.name)
          .addTo(map)
        drawnMarkers.push(waypointTwo)
      }
    })
    drawnMarkers.forEach((marker) => {
      console.log(marker.getPopup()?.getContent())
    })
  }

  map.on('mouseup', () => {
    clearTimeout(clickTimer) // Cancel the timer if mouse is released before 1 second
  })

  document.addEventListener('keydown', handleKeyPress)
  function handleKeyPress(event: KeyboardEvent) {
    const step = 0.00001
    if (engineIsOn.value) {
      switch (event.key.toLowerCase()) {
        case 'w':
          latitude.value += step
          break
        case 's':
          latitude.value -= step
          break
        case 'a':
          longitude.value -= step
          break
        case 'd':
          longitude.value += step
          break
      }
      rover.setLatLng([latitude.value, longitude.value])
    } else {
      switch (event.key.toLowerCase()) {
        case 'w':
          alert('Start the Engine to move!')
          break
        case 's':
          alert('Start the Engine to move!')
          break
        case 'a':
          alert('Start the Engine to move!')
          break
        case 'd':
          alert('Start the Engine to move!')
          break
      }
    }
  }
})
</script>

<template>

  <div class="main-view">
    <WaypointMenu
      class="waypoint-menu"
      v-if="openWaypointMenu"
      v-bind:latitude="latitudeWay"
      v-bind:longitude="longitudeWay"
      @selection="handleSelection"
    />
    <div id="map" style="width: 75%; height: 600px">Navigation map</div>
    <div class="menu-section">
      <button class="menu-section-start-button" @click="handleEngine">
        <span class="icon"></span>
        <text class="button-text">{{ engineText }}</text>
      </button>
      <div class="coordinates">
        <p>Rover location</p>
        <p>Latitude: {{ latitude }}</p>
        <p>Longitude: {{ longitude }}</p>
      </div>
      <button class="menu-section-list-button" @click="handleWayMenu">
        <text class="button-text">{{ wayMenuText }}</text>
      </button>
      <div v-if="wayListIsActive">
        <table class="container-list">
          <thead>
            <text v-if="waypoints.length <= 0"> No Waypoints</text>
            <tr v-if="waypoints.length > 0">
              <th>name</th>
              <th>Delete</th>
              <th>Rename</th>
              <th>Drive</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(entry, index) in waypoints" :key="index">
              <td>{{ entry.name }}</td>
              <td><button @click="removeWaypointFromMap(entry.name)">Delete</button></td>
              <td>
                <input type="text" v-model="entry.name" />
                <button @click="renameWaypoint(index, entry.name)">Rename</button>
              </td>
              <td>
                <button @click="driveToWaypoint(entry.latitude, entry.longitude)">Drive</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<style scoped>
.main-view {
  display: flex;
  justify-content: flex-start;
}
.menu-section {
  display: flex;
  flex-direction: column;
}
.waypoint-menu {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 999;
  background: rgba(169, 169, 169, 0.8);
}
.container-list {
  width: 600px;
  height: 350px;
  display: block;
  border: 1px solid black;
  margin: 10px;
  border-radius: 5px;
  background: rgba(169, 169, 169, 0.8);
}
.coordinates {
  width: 300px;
  height: 100px;
  border: 1px solid black;
  border-radius: 25px;
  background: rgba(169, 169, 169, 0.8);
  text-align: center;
  align-content: center;
  margin: 10px;
}
.container-coordinates p {
  margin: 10px;
}

.menu-section-start-button {
  margin: 5px;
  width: 200px;
  height: 50px;
  border: 2px solid black;
  background: rgba(169, 169, 169, 0.8);
  border-radius: 25px;
  display: flex;
  justify-content: center;
  font-size: 15px;
  position: relative;
}
.menu-section-list-button {
  margin: 5px;
  width: 200px;
  height: 50px;
  border: 1px solid black;
  background: rgba(169, 169, 169, 0.8);
  border-radius: 10px;
  display: flex;
  justify-content: center;
  font-size: 25px;
  position: relative;
}
.menu-section-list-button .button-text {
  position: absolute;
  color: black;
  font-size: 16px;
  top: 14px;
  left: 45px;
}
.menu-section-start-button .icon {
  width: 32px;
  height: 32px;
  background-image: url('/power_32_32.png');
  margin-right: 5px;
  position: absolute;
  top: 5px;
  left: 10px;
}
.menu-section-start-button .button-text {
  position: absolute;
  color: white;
  font-size: 25px;
  top: 9px;
  left: 45px;
}
.menu-section-start-button .button-text:hover {
  position: absolute;
  color: black;
  font-size: 25px;
  top: 9px;
  left: 45px;
}
</style>
