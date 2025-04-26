<template>
  <div class="svg-wrapper">
    <svg width="200" height="300" viewBox="0 0 200 300">
      <!-- سر -->
      <circle cx="100" cy="50" r="30" stroke="black" stroke-width="3" fill="none" />

      <!-- بدن -->
      <line x1="100" y1="80" x2="100" y2="200" stroke="black" stroke-width="3" />

      <!-- دست چپ -->
      <line x1="100" y1="100" x2="50" y2="150" stroke="black" stroke-width="3" />

      <!-- دست راست متحرک -->
      <line
          ref="rightArm"
          x1="100"
          y1="100"
          x2="150"
          y2="150"
          stroke="black"
          stroke-width="3"
      />

      <!-- پاها -->
      <line x1="100" y1="200" x2="70" y2="250" stroke="black" stroke-width="3" />
      <line ref="middleFoot" x1="100" y1="200" x2="40" y2="200" stroke="black" stroke-width="3" />
      <line x1="100" y1="200" x2="130" y2="250" stroke="black" stroke-width="3" />
    </svg>
    <button @click="waveHand">👋 تکون بده!</button>
  </div>
</template>

<script setup>
import {ref} from 'vue'
import gsap from 'gsap'

const middleFoot = ref(null)

function waveHand() {
  gsap.to(middleFoot.value, {
    duration: 0.3,
    attr: {x2: 140, y2: 210},
    yoyo: true,
    repeat: 10,
    ease: 'power1.inOut',
  })
}
</script>

<style scoped>
.svg-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  margin-top: 40px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>
