<template>
  <h1>Birthday App</h1>

  <div>
    <label for="dateInput">Select a Date:</label>
    <input
      id="dateInput"
      type="date"
      v-model="selectedDate"
      @change="handleDateChange"
      :max="getCurrentDate()"
    />
    <p>Selected Date: {{ selectedDate }}</p>
    <p>Your Age: {{ calculatedAge || 0 }} years</p>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const selectedDate = ref('');

const handleDateChange = () => {
  console.log('Selected Date:', selectedDate.value);
};

const calculatedAge = computed(() => {
  const birthDate = new Date(selectedDate.value);
  const currentDate = new Date();
  const ageInMilliseconds = currentDate - birthDate;
  const ageInYears = ageInMilliseconds / (365 * 24 * 60 * 60 * 1000);
  return Math.floor(ageInYears);
});

const getCurrentDate = () => {
  const currentDate = new Date();
  const year = currentDate.getFullYear();
  const month = String(currentDate.getMonth() + 1).padStart(2, '0');
  const day = String(currentDate.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
};
</script>
