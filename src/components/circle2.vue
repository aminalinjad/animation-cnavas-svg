<script setup lang="ts">
import { ref, computed, onMounted, shallowRef } from 'vue'
import { TresCanvas, useRenderLoop } from '@tresjs/core'
import { useGLTF } from '@tresjs/cientos'
import { Text3D } from '@tresjs/cientos'

const scrollY = ref(0)
const targetScrollY = ref(0)
const rotate = ref(0)
const orbitAngle = ref(0)

onMounted(() => {
  window.addEventListener('wheel', (e) => {
    targetScrollY.value += e.deltaY
    targetScrollY.value = Math.max(0, Math.min(targetScrollY.value, 1000))
  })
})

const { onLoop } = useRenderLoop()

onLoop(() => {
  scrollY.value += (targetScrollY.value - scrollY.value) * 0.01
  rotate.value += scrollY.value / 100000
  orbitAngle.value += 0.01 // سرعت گردش متن
})

const textOrbitPosition = computed(() => {
  const radius = 1.7
  const x = Math.cos(orbitAngle.value) * radius
  const z = Math.sin(orbitAngle.value) * radius + 0.1 // 👈 اینجا متن رو میاریم جلوتر
  const y = sphereY.value + 0.2
  return [x, y, z]
})

const sphereScale = computed(() => {
  const s = 1.7 - scrollY.value / 800
  return Math.max(s, 0.5)
})

const sphereY = computed(() => -3 + Math.min(scrollY.value / 1000, 1) * 3)

const textY = computed(() => 2 - Math.min(scrollY.value / 300, 5))

const { scene } = await useGLTF('/models/planet.glb')
const model = shallowRef(scene)
</script>

<template>
  <div class="wrapper">
    <TresCanvas clear-color="#0e0e0e" window-size>
      <TresAmbientLight :intensity="1" />
      <TresDirectionalLight :position="[3, 3, 3]" :intensity="0.5" />

      <Suspense>
        <Text3D
            :position="textOrbitPosition"
            :fontSize="0.2"
            color="#FF6347"
            font="https://raw.githubusercontent.com/Tresjs/assets/main/fonts/FiraCodeRegular.json"
        >
          The New World
        </Text3D>
      </Suspense>
      <!-- مدل -->
      <primitive
          :rotation="[0, rotate, 0]"
          :object="model"
          :scale="[sphereScale, sphereScale, sphereScale]"
          :position="[0, sphereY, 0]"
      />

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
  height: 500vh;
  background: linear-gradient(#ddeeff, #aaccff);
}
</style>
