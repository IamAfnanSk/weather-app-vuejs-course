<template>
  <div class="weather-card-container">
    <Weather
      data-test="weather-card"
      v-for="(location, idx) in locations"
      :key="idx"
      @deleteLocation="() => deleteLocation(idx)"
      :location="location"
    />

    <EmptyWeatherButton
      data-test="weather-new-button"
      @click="showNewLocationModal = true"
    />
  </div>

  <NewLocationModal
    v-if="showNewLocationModal"
    data-test="weather-new-modal"
    @addNewLocation="addNewLocation"
  />

  <div
    v-if="showNewLocationModal"
    data-test="weather-new-modal-overlay"
    @click="showNewLocationModal = false"
    class="modal-wrapper"
  ></div>
</template>

<script setup>
import Weather from "./components/Weather.vue";
import EmptyWeatherButton from "./components/EmptyWeatherButton.vue";
import NewLocationModal from "./components/NewLocationModal.vue";
import { ref } from "vue";

const locations = ref(["Mumbai", "NY"]);

const showNewLocationModal = ref(false);

function addNewLocation(location) {
  locations.value.push(location);
  showNewLocationModal.value = false;
}

function deleteLocation(idx) {
  locations.value.splice(idx, 1);
}
</script>

<style scoped>
.weather-card-container {
  display: grid;
  grid-template-columns: 1fr;
  gap: 20px;
  padding: 30px;
}

@media only screen and (min-width: 700px) {
  .weather-card-container {
    grid-template-columns: 1fr 1fr;
  }
}

@media only screen and (min-width: 1200px) {
  .weather-card-container {
    grid-template-columns: 1fr 1fr 1fr;
  }
}

@media only screen and (min-width: 1600px) {
  .weather-card-container {
    grid-template-columns: 1fr 1fr 1fr 1fr;
  }
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
