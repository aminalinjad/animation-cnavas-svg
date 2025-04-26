<script setup>
import {ref, watchEffect} from 'vue'
import {
  TresCanvas,
  useLoader,
  useRenderLoop
} from '@tresjs/core'
import {GLTFLoader} from 'three/addons/loaders/GLTFLoader'
import * as THREE from 'three'
import {OrbitControls} from "@tresjs/cientos";

// مدل رو لود می‌کنیم
const {scene} = await useLoader(GLTFLoader, '/models/cubone.glb')

// ریفرنس‌ها
const knifeObject = scene.getObjectByName('group1868352797') // همون چاقو یا جزئی که می‌خوای کنترلش کنی
const rootObject = scene // کل مدل

// مقدار هدف بر اساس اسکرول
const scrollTarget = ref(0)

// گوش می‌کنیم به اسکرول
window.addEventListener('scroll', () => {
  const scrollY = window.scrollY / 200
  scrollTarget.value = scrollY
})

// هر فریم یک مقدار نزدیک بشه به مقدار نهایی
useRenderLoop().onLoop(() => {
  if (knifeObject) {
    // چاقو کمی بالا پایین بره
    knifeObject.rotation.z = THREE.MathUtils.lerp(knifeObject.rotation.z, scrollTarget.value, 0.1)
  }

  if (rootObject) {
    // کل مدل کمی بچرخه به راست و چپ
    rootObject.rotation.y = THREE.MathUtils.lerp(rootObject.rotation.y, scrollTarget.value, 0.1)
  }
})
</script>

<template>
  <TresCanvas window-size clear-color="#000">
    <TresPerspectiveCamera :position="[0, 0, 5]"/>
    <TresAmbientLight :intensity="1"/>
    <TresDirectionalLight :position="[5, 5, 5]"/>
    <primitive :rotation="[0, 20, 0]" :object="scene"/>
<!--    <OrbitControls/>-->
  </TresCanvas>
</template>
