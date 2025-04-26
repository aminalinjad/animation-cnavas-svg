<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'
import { TresCanvas } from '@tresjs/core'
import { useRenderLoop } from '@tresjs/core'

const scrollY = ref(0)
const targetScrollY = ref(0)

onMounted(() => {
  window.addEventListener('wheel', (e) => {
    targetScrollY.value += e.deltaY
    // محدودش کن به یه رنج مناسب
    targetScrollY.value = Math.max(0, Math.min(targetScrollY.value, 1000))
  })
})

// انیمیشن نرم با render loop
const { onLoop } = useRenderLoop()

onLoop(() => {
  scrollY.value += (targetScrollY.value - scrollY.value) * 0.01
})

// scale بین 1.5 تا 0.5
const sphereScale = computed(() => {
  const s = 1.5 - scrollY.value / 1000
  return Math.max(s, 0.5)
})

// y از -3 تا 1
const sphereY = computed(() => -3 + Math.min(scrollY.value / 1000, 1) * 3)
</script>

<template>
  <div class="wrapper">
    <TresCanvas clear-color="#aaccee" window-size>
      <!-- نور -->
      <TresAmbientLight :intensity="1" />
      <TresDirectionalLight :position="[3, 3, 3]" :intensity="0.5" />

      <!-- گوی -->
      <TresMesh :scale="[sphereScale, sphereScale, sphereScale]" :position="[0, sphereY, 0]">
        <TresSphereGeometry :args="[3, 32, 32]" />
        <TresMeshStandardMaterial color="hotpink" />
      </TresMesh>

      <!-- <OrbitControls /> -->
    </TresCanvas>
  </div>
</template>

<style>
body {
  margin: 0;
  overflow: hidden;
}
.wrapper {
  width: 100vw;
  height: 200vh;
  background: linear-gradient(#ddeeff, #aaccff);
}
</style>
