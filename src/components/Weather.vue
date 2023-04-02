<template>
  <div class="weather-card">
    <div class="close-button" @click="$emit('deleteLocation')">delete</div>

    <div class="weather-card-section" v-if="weatherData">
      <div class="weather-card-section-item">
        <p class="weather-p">Now</p>
      </div>
      <div class="weather-card-section-item weather-actual">
        <div>
          <p class="weather-p">{{ weatherData.temp_c }} °C</p>
        </div>
        <div>
          <img
            :src="weatherData.condition.icon"
            alt="weather image"
            class="weather-card-img"
          />
        </div>
      </div>
      <div class="weather-card-section-item">
        <p class="weather-p">{{ weatherData.condition.text }}</p>
      </div>
    </div>
    <div class="weather-card-section" v-else-if="locationFetchError">
      Error fetching weather data
    </div>
    <div class="weather-card-section" v-else>Loading...</div>

    <div class="weather-card-section weather-location" v-if="locationData">
      <div class="weather-card-section-item">
        <p class="weather-p">{{ localTime }}</p>
      </div>
      <div class="weather-card-section-item">
        <p class="weather-p">
          {{ locationData.name }}, {{ locationData.region }}
        </p>
      </div>
    </div>
    <div class="weather-card-section" v-else-if="locationFetchError">
      <p>{{ locationFetchError }}</p>
    </div>
    <div class="weather-card-section" v-else>Loading...</div>
  </div>
</template>

<script setup>
import { toRefs, watchEffect, ref, computed } from "vue";

const props = defineProps({
  location: {
    type: String,
    required: true,
  },
});

const emits = defineEmits(["deleteLocation"]);

const { location } = toRefs(props);

const locationData = ref(null);
const weatherData = ref(null);

const locationFetchError = ref(false);

const BASE_URL = "http://api.weatherapi.com/v1";
const KEY = "API_KEY_HERE";

const localTime = computed(() => {
  return locationData.value.localtime
    ? new Date(locationData.value.localtime).toLocaleString()
    : null;
});

watchEffect(async () => {
  try {
    locationFetchError.value = false;
    const res = await fetch(
      `${BASE_URL}/current.json?q=${location.value}&key=${KEY}`
    );
    const data = await res.json();

    if (data.error) {
      locationFetchError.value = data.error.message;
      return;
    }

    locationData.value = data.location;
    weatherData.value = data.current;
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
