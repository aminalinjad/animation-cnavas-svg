<script setup lang="ts">
import { onMounted } from 'vue'
import Lenis from '@studio-freight/lenis'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

onMounted(() => {
  // راه اندازی Lenis
  const lenis = new Lenis({})

  function raf(time: number) {
    lenis.raf(time)
    requestAnimationFrame(raf)
  }

  requestAnimationFrame(raf)

  // اتصال gsap و ScrollTrigger
  gsap.registerPlugin(ScrollTrigger)

  // انیمیشن زوم و تغییر بکگراند
  gsap.timeline({
    scrollTrigger: {
      trigger: '.zoom-section',
      start: 'top top',
      end: '+=200%',
      scrub: true,
      pin: true,
      markers: true, // برای دیباگ
    }
  })
      .to('.hi-text', {
        scale: 150,
        xPercent: -300,
        yPercent: -10,
        ease: 'power2.inOut'
      }, 0)
      .to('.zoom-section', {
        backgroundColor: '#ffffff'
      }, 0.5)
})
</script>

<template>
  <div class="container2">
    <!-- سکشن زوم -->
    <section class="zoom-section">
      <h1 class="hi-text">HI</h1>
    </section>

    <!-- سکشن دوم -->
    <section class="normal-section">
      <h2>this is second section</h2>
      <p>keif kardi ya na</p>
    </section>
  </div>
</template>

<style scoped>
.container2 {
  overflow: hidden;
}

.zoom-section {
  height: 100vh;
  background-color: black;
  display: flex;
  align-items: center;
  justify-content: center;
}

.hi-text {
  font-size: 10vw;
  color: white;
  font-weight: bold;
}

.normal-section {
  height: 100vh;
  background-color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  color: red;
  z-index: 999;
  position: relative;
}
</style>
