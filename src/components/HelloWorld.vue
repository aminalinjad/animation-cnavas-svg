<template>
  <TresCanvas window-size clear-color="#111">
    <TresPerspectiveCamera :position="[0, 0, 10]" />
    <TresAmbientLight :intensity="1" />
    <TresDirectionalLight :position="[5, 5, 5]" />

    <TresMesh :position="[0, cubeY, 0]" :rotation="[cubeRotation, cubeRotation, 0]">
      <TresBoxGeometry :args="[2, 2, 2]" />
      <TresMeshStandardMaterial color="hotpink" />
    </TresMesh>
  </TresCanvas>
</template>

<script setup>
import { ref, onMounted } from 'vue'

// موقعیت مکعب و چرخشش
const cubeY = ref(0)
const cubeRotation = ref(0)

onMounted(() => {
  window.addEventListener('scroll', () => {
    const scroll = window.scrollY

    // مکعب با اسکرول به بالا می‌ره
    cubeY.value = scroll / 100

    // و یه کم هم می‌چرخه
    cubeRotation.value = scroll / 200
  })
})
</script>

<style>
html, body, #app {
  margin: 0;
  padding: 0;
  height: 200vh; /* طول صفحه رو زیاد می‌کنیم برای اسکرول */
  background-color: #111;
}
</style>