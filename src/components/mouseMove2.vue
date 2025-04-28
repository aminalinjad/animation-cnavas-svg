<script setup>
import { ref, onMounted } from 'vue'
import {
  TresCanvas,
  useRenderLoop
} from '@tresjs/core'
import * as THREE from 'three'

const cameraRef = ref(null)

// موقعیت هدف موس
const target = ref(new THREE.Vector3(0, 0, 0))

// موقعیت‌های مستقل برای چند مکعب
const cubes = [
  ref(new THREE.Vector3(0, 0, 0)),
  ref(new THREE.Vector3(-5, 1, 0)),
  ref(new THREE.Vector3(10, -1, 0)),
  ref(new THREE.Vector3(5, -1, 0)),
  ref(new THREE.Vector3(-4, 1, 0)),
]

// گرفتن موقعیت موس
onMounted(() => {
  window.addEventListener('mousemove', (e) => {
    const mouse = new THREE.Vector2(
        (e.clientX / window.innerWidth) * 2 - 1,
        -(e.clientY / window.innerHeight) * 2 + 1
    )

    const raycaster = new THREE.Raycaster()
    raycaster.setFromCamera(mouse, cameraRef.value)

    const plane = new THREE.Plane(new THREE.Vector3(0, 0, 1), 0)
    const point = new THREE.Vector3()
    raycaster.ray.intersectPlane(plane, point)

    target.value = point
  })
})

// انیمیشن حرکت مکعب‌ها
useRenderLoop().onLoop(() => {
  cubes.forEach((cube, i) => {
    const speed = 0.01 + i * 0.01 // هر مکعب سرعت متفاوتی داره
    cube.value.lerp(target.value, speed)
  })
})
</script>

<template>
  <TresCanvas clear-color="#111" window-size>
    <TresPerspectiveCamera ref="cameraRef" :position="[0, 0, 10]"/>
    <TresAmbientLight :intensity="1"/>
    <TresDirectionalLight :position="[5, 5, 5]"/>

    <!-- چند مکعب با رنگ متفاوت -->
    <TresMesh
        v-for="(cube, index) in cubes"
        :key="index"
        :position="[cube.value.x, cube.value.y, cube.value.z]"
    >
      <TresSphereGeometry :args="[1, 1, 1, 1, 1, 1]"/>
      <TresMeshStandardMaterial :color="`hsl(${index * 70}, 100%, 60%)`"/>
    </TresMesh>
  </TresCanvas>
</template>
