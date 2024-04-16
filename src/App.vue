<script setup lang="ts">
import { RouterView } from 'vue-router'
function instructions() {
  alert(
    'To  drive the rover switch on the engine from the Start Engine button at the top right\n\n' +
      ' While the engin is running use the keyboard A, W, S, and D keys to navigate the rover.\n' +
      'By holding down the mouse you can create a waypoint \n' +
      '1) Drive - fast-travel the rover to the location\n' +
      '2) Save - saves the waypoint to the list of waypoinst or\n' +
      '3) Discard -Cancel the operation by clicking  \n\n' +
      'The Waypoints List provides a view of all saved waypoints with the following options\n' +
      '1) Delete - removes the waypoint from the Waypoints List \n' +
      '2) Rename - Enter the desired new name in the text box and click "Rename" to rename the waypoint \n' +
      '3) Drive  - fast-travels the rove to the location'
  )
}
</script>

<template>
  <div class="container">
    <nav>
      <text class="text" @click="instructions">INSTRUCTION</text>
    </nav>
  </div>

  <div class="content">
    <RouterView />
  </div>
</template>

<style scoped>
@import url('https://unpkg.com/leaflet@1.9.4/dist/leaflet.css');

.container {
  display: flex;
  margin: auto;
  justify-content: center;
}

.text {
  border: 1px solid black;
  border-radius: 5px;
  margin: 10px;
  padding: 10px;
}
.text:hover {
  background-color: #f0f0f0;
}
.content {
  display: block;
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

nav {
  width: auto;
  font-size: 12px;
  text-align: center;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: 100px;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
