<template>
  <div class="svg-wrapper">
    <svg width="200" height="300" viewBox="0 0 200 300">
      <!-- سر -->
      <circle cx="100" cy="60" r="30" stroke="black" fill="none" stroke-width="3" />

      <!-- چشم‌ها -->
      <circle cx="90" cy="55" r="3" fill="black" />
      <circle cx="110" cy="55" r="3" fill="black" />

      <!-- دهن -->
      <path d="M90,70 Q100,80 110,70" stroke="black" fill="none" stroke-width="2"/>

      <!-- بدن -->
      <line x1="100" y1="90" x2="100" y2="180" stroke="black" stroke-width="3" />

      <!-- پای چپ -->
      <line x1="100" y1="180" x2="80" y2="250" stroke="black" stroke-width="3" />

      <!-- پای راست -->
      <line x1="100" y1="180" x2="120" y2="250" stroke="black" stroke-width="3" />

      <!-- دست متحرک -->
      <path
          ref="armPath"
          :d="currentArmPath"
          stroke="black"
          fill="none"
          stroke-width="3"
          @click="waveHand"
          style="cursor: pointer;"
      />
    </svg>
    <p>روی دست کلیک کن تا تکون بخوره ✋</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import gsap from 'gsap'

// دو مسیر مختلف برای دست: پایین و بالا
const armDown = 'M100,100 Q120,110 140,130'  // دست پایین
const armUp = 'M100,100 Q120,60 140,30'      // دست بالا

const currentArmPath = ref(armDown)
const armPath = ref(null)

let isUp = false

const waveHand = () => {
  const nextPath = isUp ? armDown : armUp
  isUp = !isUp

  gsap.to(armPath.value, {
    duration: 0.8,
    attr: { d: nextPath },
    ease: 'power1.inOut',
    onUpdate: () => {
      currentArmPath.value = armPath.value.getAttribute('d')
    },
  })
}
</script>

<style scoped>
.svg-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 40px;
}
</style>
