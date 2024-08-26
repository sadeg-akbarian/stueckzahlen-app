<script setup>
import HelloWorld from "./components/HelloWorld.vue";
</script>
/
<template>
  <div class="whole-area">
    <HelloWorld msg="Stückzahlen - App" />
  </div>
</template>

<style scoped>
*,
::before,
::after {
  box-sizing: border-box;
}

.whole-area {
  margin: 0;
  height: 100vh;
}
</style>
