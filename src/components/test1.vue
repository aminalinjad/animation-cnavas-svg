<template>
  <section style="display: flex" class="hero-section">
    <div class="text-content">
      <h1>Create awesome 3D experiences with Vue</h1>
    </div>
    <div class="canvas-container">
      <TresCanvas windowSize  preset="realistic" width="2568" height="500">
        <TresPerspectiveCamera fav="60" :position="[1, 0, 6]" :look-at="[0, 0, 0]" />
        <TresMesh :rotation="[rotationY, 0, 0]" :position="[positionX, positionY, 0]" :scale="[scale, scale, scale]">
          <TresBoxGeometry :args="[.5 ,.5 ,.5]" />
          <TresMeshStandardMaterial color="skyblue" />
        </TresMesh>
        <TresAmbientLight :intensity="0.8" />
        <TresDirectionalLight :position="[2, 2, 2]" />
      </TresCanvas>
    </div>
  </section>
</template>
<script setup>
import {TresCanvas} from "@tresjs/core";
import { ref, onMounted } from 'vue'
import { useRenderLoop } from '@tresjs/core'

const scrollY = ref(0)
const rotationY = ref(0)
const positionX = ref(2)
const positionY = ref(1.5)
const scale = ref(1)

onMounted(() => {
  window.addEventListener('scroll', () => {
    scrollY.value = window.scrollY
  })
})

const { onLoop } = useRenderLoop()
onLoop(() => {
  // چرخش مکعب بر اساس اسکرول
  rotationY.value = scrollY.value / 300
  positionX.value = 2 -(scrollY.value / 100)
  positionY.value =1.5  -(scrollY.value / 300)
  scale.value = 2 +scrollY.value / 400


  // حرکت مکعب از سمت چپ به راست
  // positionX.value = (scrollY.value / window.innerHeight) // این مقدار را تنظیم کنید که مکعب از کجا خارج بشه
})
</script>
<style>
body {
  margin: 0;
  font-family: sans-serif;
  overflow-x: hidden;
}

.hero-section {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 5vw;
  box-sizing: border-box;
  position: relative;
}

.text-content {
  flex: 1;
  padding-right: 2vw;
}

.canvas-container {
  flex: 1;
}

</style>
