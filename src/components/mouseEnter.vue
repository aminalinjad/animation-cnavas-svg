<script setup>
import { ref, onMounted, watch } from 'vue'
import {
  TresCanvas,
} from '@tresjs/core'

// متغیر برای تغییر موقعیت مکعب
const shaking = ref(false)
const x = ref(0)
const y = ref(0)

// زمانی که موس وارد مکعب می‌شود، این متد اجرا می‌شود
const handlePointerEnter = (event) => {
  console.log(event.movementX)
  if (event.movementX >= 0) {
    x.value = x.value = 0.3
  }else {
    x.value = x.value - 0.3
  }

  if (event.movementY >= 0) {
    y.value = y.value - 0.3
  }else {
    y.value = y.value + 0.3
  }

  shaking.value = true
  //
  setTimeout(() => {
    shaking.value = false // بعد از 1 ثانیه، مکعب به موقعیت اولیه برمی‌گردد
  }, 200)
}

// به محض اینکه shaking تغییر کنه، موقعیت مکعب تغییر پیدا می‌کنه
onMounted(() => {
})
</script>

<template>
  <TresCanvas window-size clear-color="#222">
    <!-- دوربین -->
    <TresPerspectiveCamera :position="[0, 0, 10]" />
    <TresAmbientLight :intensity="1" />
    <TresDirectionalLight :position="[5, 5, 5]" />

    <!-- مکعب که وقتی موس بهش می‌خورد تکان می‌خورد -->
    <TresMesh
        ref="cubeMesh"
        :position="[x, y, 0]"
        @pointer-enter="handlePointerEnter"
    >
      <TresBoxGeometry :args="[2, 2, 2]" />
      <TresMeshStandardMaterial color="orange" />
    </TresMesh>
  </TresCanvas>
</template>
