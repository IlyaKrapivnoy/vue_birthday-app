<template>
  <h1>Birthday App</h1>

  <div>
    <label for="dateInput">Select a Date:</label>
    <input
      id="dateInput"
      type="date"
      v-model="selectedDate"
      @change="handleDateChange"
    />
    <p>Selected Date: {{ selectedDate }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const selectedDate = ref('');

const handleDateChange = () => {
  console.log('Selected Date:', selectedDate.value);
};
</script>

<style scoped></style>
