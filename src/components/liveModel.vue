<script setup>
import {TresCanvas} from '@tresjs/core'
import {OrbitControls, GLTFModel} from '@tresjs/cientos'
</script>

<template>
  <TresCanvas window-size clear-color="#000">
    <!-- دوربین -->
    <TresPerspectiveCamera :position="[0, 1, 5]"/>

    <!-- نور -->
    <TresAmbientLight :intensity="0.8"/>
    <TresDirectionalLight :position="[5, 5, 5]" :intensity="1"/>

    <!-- مدل -->
    <Suspense>
      <GLTFModel path="/models/cubone.glb" :scale="[1.5, 1.5, 1.5]"/>
    </Suspense>

    <!-- کنترل چرخش با موس -->
    <OrbitControls/>
  </TresCanvas>
</template>
