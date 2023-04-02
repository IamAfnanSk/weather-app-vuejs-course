<template>
  <div class="weather-card">
    <p class="icon">+</p>
    <p>Add new location card</p>
  </div>
</template>

<script setup></script>

<style scoped>
.weather-card {
  cursor: pointer;
  border-radius: 5px;
  box-shadow: 1px 1px 5px 2px #ccc;
  padding: 20px 30px;
}

.weather-card {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

.icon {
  font-size: 7rem;
  font-weight: 100;
  color: #565656;
}
</style>
