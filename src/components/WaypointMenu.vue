<script setup lang="ts">
import { ref, defineEmits } from 'vue'

const selection = ref(0)

const emitSelection = defineEmits(['selection'])

const driveToWaypoint = () => {
  selection.value = 1
  emitSelection('selection', selection.value)
}

const saveConfirmation = () => {
  selection.value = 2
  emitSelection('selection', selection.value)
}

const discard = () => {
  selection.value = 3
  emitSelection('selection', selection.value)
}
</script>

<template>
  <div class="container">
    <p class="p">Select operation</p>
    <div>
      <button @click="driveToWaypoint()">Drive</button>
      <button @click="saveConfirmation()">Save</button>
      <button @click="discard()">Discard</button>
    </div>
  </div>
</template>

<style scoped>
.container {
  width: 500px;
  height: 250px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  border: 2px solid black;
  border-radius: 10px;
  background-color: #f0f0f0;
}

.container button {
  margin: 5px;
}
.container p {
  margin-bottom: 25px;
}
</style>
