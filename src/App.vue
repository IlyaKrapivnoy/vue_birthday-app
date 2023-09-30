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
    <label for="timeInput">Enter Time (HH:mm):</label>
    <input
      id="timeInput"
      type="time"
      v-model="selectedTime"
      @input="handleTimeChange"
    />
    <p>Your Age:</p>
    <p>
      {{ calculatedAge.years || 0 }} years,
      {{ calculatedAge.months || 0 }} months, {{ calculatedAge.days || 0 }} days
    </p>
  </div>
</template>

<script setup>
import { computed, ref } from 'vue';

const selectedDate = ref('');
const selectedTime = ref('');

const handleDateChange = () => {
  console.log('Selected Date:', selectedDate.value);
};

const handleTimeChange = () => {
  console.log('Selected Time:', selectedTime.value);
};

const calculatedAge = computed(() => {
  const birthDate = new Date(selectedDate.value);
  const currentDate = new Date();
  let years = currentDate.getFullYear() - birthDate.getFullYear();
  let months = currentDate.getMonth() - birthDate.getMonth();
  let days = currentDate.getDate() - birthDate.getDate();

  if (days < 0) {
    months -= 1;
    days += new Date(
      currentDate.getFullYear(),
      currentDate.getMonth(),
      0
    ).getDate();
  }
  if (months < 0) {
    years -= 1;
    months += 12;
  }

  return {
    years,
    months,
    days
  };
});

const getCurrentDate = () => {
  const currentDate = new Date();
  const year = currentDate.getFullYear();
  const month = String(currentDate.getMonth() + 1).padStart(2, '0');
  const day = String(currentDate.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
};
</script>
