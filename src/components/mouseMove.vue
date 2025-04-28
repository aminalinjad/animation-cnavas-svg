<script setup>
import { ref, onMounted } from 'vue'
import {
  TresCanvas,
} from '@tresjs/core'

const cubePosition = ref([0, 0, 0]) // موقعیت مکعب

onMounted(() => {
  window.addEventListener('mousemove', (e) => {
    // موقعیت موس رو می‌گیریم
    const x = (e.clientX / window.innerWidth) * 2 - 1
    const y = -(e.clientY / window.innerHeight) * 2 + 1

    // تنظیم موقعیت مکعب بر اساس موقعیت موس
    cubePosition.value = [x * 5, y * 5, -3]
  })
})
</script>

<template>
  <TresCanvas window-size clear-color="#222">
    <!-- دوربین -->
    <TresPerspectiveCamera :position="[0, 0, 10]"/>
    <TresAmbientLight :intensity="1"/>
    <TresDirectionalLight :position="[5, 5, 5]"/>

    <!-- مکعب که با موس حرکت می‌کنه -->
    <TresMesh :position="cubePosition">
      <TresBoxGeometry :args="[2, 2, 2]"/>
      <TresMeshStandardMaterial color="orange"/>
    </TresMesh>
  </TresCanvas>
</template>
