<template>
  <div class="weather-card">
    <div class="close-button" data-test="delete-weather">delete</div>

    <!-- Show below div if weatherData exist -->
    <div
      v-if="weatherData"
      class="weather-card-section"
      data-test="weather-section-success"
    >
      <div class="weather-card-section-item">
        <p class="weather-p">Now</p>
      </div>
      <div class="weather-card-section-item weather-actual">
        <div>
          <!-- Add actual weather in mustache syntax -->
          <p class="weather-p" data-test="weather-celsius">
            {{ weatherData.temp_c }} °C
          </p>
        </div>
        <div data-test="weather-image">
          <!-- add condition image src -->
          <img
            :src="weatherData.condition.icon"
            alt="weather image"
            class="weather-card-img"
          />
        </div>
      </div>
      <div class="weather-card-section-item">
        <!-- Show condition text here: refer example json -->
        <p class="weather-p" data-test="weather-condition">
          {{ weatherData.condition.text }}
        </p>
      </div>
    </div>
    <!-- else if locationFetchError exist show below div -->
    <div
      v-else-if="locationFetchError"
      class="weather-card-section"
      data-test="weather-section-error"
    >
      Error fetching weather data
    </div>
    <!-- else show below div -->
    <div
      v-else
      class="weather-card-section"
      data-test="weather-section-loading"
    >
      Loading...
    </div>

    <!-- show below div if locationData exist -->
    <div
      v-if="locationData"
      class="weather-card-section weather-location"
      data-test="location-section-success"
    >
      <div class="weather-card-section-item">
        <p class="weather-p">{{ localTime || "" }}</p>
      </div>
      <div class="weather-card-section-item">
        <p class="weather-p" data-test="weather-location">
          <!-- location name (comma) location region -->
          {{ locationData.name }}, {{ locationData.region }}
        </p>
      </div>
    </div>
    <!-- else if locationFetchError show this div -->
    <div
      v-else-if="locationFetchError"
      class="weather-card-section"
      data-test="location-section-error"
    >
      <!-- show locationFetchError in below p -->
      <p>{{ locationFetchError }}</p>
    </div>
    <!-- else show below div -->
    <div
      v-else
      class="weather-card-section"
      data-test="location-section-loading"
    >
      Loading...
    </div>
  </div>
</template>

<script setup>
import { toRefs, watchEffect, ref, computed } from "vue";
import fetch from "cross-fetch";

const props = defineProps({
  location: {
    type: String,
    required: true,
  },
});

const { location } = toRefs(props);

const locationData = ref(null);
const weatherData = ref(null);

const locationFetchError = ref(false);

const BASE_URL = "https://api.weatherapi.com/v1";
const KEY = "YOUR_API_KEY_HERE";

const localTime = computed(() => {
  return locationData.value?.localtime
    ? new Date(locationData.value.localtime).toLocaleString()
    : null;
});

watchEffect(async () => {
  locationFetchError.value = false;
  locationData.value = null;
  weatherData.value = null;

  try {
    const res = await fetch(
      `${BASE_URL}/current.json?q=${location.value}&key=${KEY}`
    );
    const data = await res.json();

    if (data.error) {
      locationFetchError.value = data.error.message || "Error";
    } else {
      locationData.value = data.location;
      weatherData.value = data.current;
    }
  } catch (error) {
    locationFetchError.value = error.message;
  }
});
</script>

<style scoped>
.close-button {
  position: absolute;
  right: 10px;
  top: 10px;
  border-radius: 5px;
  background: #000;
  color: white;
  font-size: 0.75rem;
  padding: 2px 6px;
  cursor: pointer;
}

.weather-card {
  position: relative;
  border-radius: 5px;
  box-shadow: 1px 1px 5px 2px #ccc;
  padding: 20px 30px;
}

.weather-card {
  display: flex;
  flex-direction: column;
}

.weather-card-section:first-child {
  padding-bottom: 10px;
}
.weather-card-section:last-child {
  border-top: 1px solid #e9e9e9;
  padding-top: 10px;
}

.weather-actual {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px 0;
}

.weather-actual .weather-p {
  font-size: 2rem;
}

.weather-location {
  color: #5b5b5b;
}
</style>
