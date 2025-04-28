<template>
  <div class="svg-wrapper">
    <svg width="200" height="200" viewBox="0 0 200 200">
      <circle
          cx="100"
          cy="100"
          :r="isBig ? 60 : 40"
          :fill="isBig ? 'tomato' : 'skyblue'"
          class="circle"
          @click="toggleSize"
      />
    </svg>
    <p>روی دایره کلیک کن 🎯</p>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const isBig = ref(false)

function toggleSize() {
  isBig.value = !isBig.value
}
</script>

<style scoped>
.svg-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 50px;
}

.circle {
  transition: all 0.4s ease;
  cursor: pointer;
}
</style>
