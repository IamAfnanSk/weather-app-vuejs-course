<template>
  <div class="modal">
    <h1>Add a new location</h1>

    <input
      type="text"
      v-model="location"
      placeholder="Add search query, like a city name"
      data-test="weather-new-add-input"
    />
    <button data-test="weather-new-add-button" @click="emitNewLocation">
      Add
    </button>
  </div>
</template>

<script setup>
import { ref } from "vue";

const emits = defineEmits(["addNewLocation"]);

const location = ref("");

function emitNewLocation() {
  emits("addNewLocation", location.value);
  location.value = "";
}
</script>

<style scoped>
.modal {
  background: #fff;
  z-index: 2;
  position: fixed;
  width: 450px;
  padding: 20px 30px;
  border-radius: 10px;
  box-shadow: 1px 1px 10px #ccc;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  gap: 20px;
  display: flex;
  flex-direction: column;
}

input {
  outline: none;
  border: 1px solid #ccc;
  width: 100%;
  border-radius: 5px;
  padding: 5px 10px;
}
</style>
