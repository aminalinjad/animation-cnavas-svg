<script setup>
import test1 from "./components/test1.vue";
import test2 from "./components/test2.vue";

</script>

<template>
<!--  <test1/>-->
  <Suspense>
    <test2/>
  </Suspense>


  <div style="height: 8000px"></div>
</template>

<style scoped>

</style>
