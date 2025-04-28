<template>
  <div class="svg-wrapper">
    <svg width="400" height="200" viewBox="0 0 400 200">
      <!-- مسیر -->
      <path
          id="thePath"
          d="M 50 150 Q 200 0 350 150"
          fill="none"
          stroke="gray"
          stroke-width="2"
      />

      <!-- دایره‌ای که قراره روی مسیر حرکت کنه -->
      <circle
          ref="ball"
          r="10"
          fill="purple"
      />
    </svg>

    <button @click="move">حرکت کن!</button>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import gsap from 'gsap'
import { MotionPathPlugin } from 'gsap/MotionPathPlugin'

gsap.registerPlugin(MotionPathPlugin)

const ball = ref(null)

const move = () => {
  gsap.to(ball.value, {
    duration: 2,
    repeat: 0,
    ease: 'power1.inOut',
    motionPath: {
      path: '#thePath',
      align: '#thePath',
      autoRotate: true,
      alignOrigin: [0.5, 0.5]
    }
  })
}
</script>

<style scoped>
.svg-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 30px;
}

button {
  margin-top: 20px;
  padding: 8px 16px;
  border: none;
  background-color: purple;
  color: white;
  border-radius: 6px;
  cursor: pointer;
}
</style>
