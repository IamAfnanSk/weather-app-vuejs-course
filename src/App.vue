<template>
  <div class="weather-card-container">
    <Weather :location="'Mumbai'" />
  </div>
</template>

<script setup>
import Weather from "./components/Weather.vue";
</script>

<style scoped>
.weather-card-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 20px;
  padding: 30px;
}

.modal-wrapper {
  width: 100vw;
  height: 100vh;
  position: fixed;
  background: #ccc;
  opacity: 0.6;
  top: 0;
  left: 0;
}
</style>
