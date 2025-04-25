<script setup>
import {TresCanvas, useRenderLoop} from '@tresjs/core'
import {OrbitControls} from '@tresjs/cientos'
import {onMounted, ref} from 'vue'
import {useAnimations, useGLTF} from '@tresjs/cientos'
import * as THREE from 'three'

const {onLoop} = useRenderLoop()
const {scene: model} = await useGLTF('/Dice.glb')
const {scene: model2} = await useGLTF('/Dice.glb')
const {scene: deskModel} = await useGLTF('/desk.glb')

const rotation = ref(0)
const translateY = ref(0)
const scrollY = ref(0)
const boxDice = new THREE.Box3()
const boxDesk = new THREE.Box3()
const hasCollided = ref(false)
onMounted(() => {
  window.addEventListener('scroll', () => {
    scrollY.value = window.scrollY
  })
})


onLoop(() => {
  if (!hasCollided.value || scrollY.value < 600) {
    translateY.value = 1 - scrollY.value / 600
    rotation.value = scrollY.value / 300
  }

  // آپدیت پوزیشن مدل‌ها
  // model.position.set(0.5, translateY.value, 0)

  // BoundingBox
  boxDice.setFromObject(model)
  boxDesk.setFromObject(deskModel)

  if (boxDice.intersectsBox(boxDesk)) {
    hasCollided.value = true
    model.rotation.set(-0.2, 2, 1)
    model2.rotation.set(-0.2, 2.5, 2.8)
    console.log('✅ برخورد و توقف افتادن')
  }else {
    hasCollided.value = false
  }
})

</script>

<template>
  <div class="wrapper">
    <TresCanvas clear-color="#111111" window-size>
      <TresPerspectiveCamera :position="[0, 1, 3]" :look-at="[0, 0, 0]"/>
<!--      <OrbitControls/>-->
      <TresAmbientLight :intensity="1"/>
      <TresDirectionalLight :position="[2, 2, 2]" :intensity="1"/>

      <!-- مکعب -->
      <primitive :scale="[0.5, 0.5, 0.5]" :rotation="[-0.2, rotation, 1]" :position="[0.3, translateY,0.5]" :object="model"/>
      <primitive :scale="[0.5, 0.5, 0.5]" :rotation="[0, rotation, 0]" :position="[0, translateY,0.5]" :object="model2"/>
      <primitive :scale="[1.3, 1.3, 1.3]"  :rotation="[0, 0, 0]" :position="[0, -1 ,0]" :object="deskModel"/>
    </TresCanvas>
  </div>
</template>

<style scoped>
.wrapper {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

.face {
  font-size: 24px;
  font-weight: bold;
  background: white;
  color: black;
  border-radius: 8px;
  padding: 6px 12px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.4);
}
</style>
