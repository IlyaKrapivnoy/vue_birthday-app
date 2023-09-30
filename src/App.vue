<template>
  <h1 style="font-size: 100px">Birthday App</h1>

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
    <p>Selected Time: {{ selectedTime }}</p>
    <p>Your Age:</p>
    <p>
      {{ calculatedAge.years || 0 }} years,
      {{ calculatedAge.months || 0 }} months,
      {{ calculatedAge.days || 0 }} days, {{ calculatedAge.hours || 0 }} hours,
      {{ calculatedAge.minutes || 0 }} minutes,
      {{ calculatedAge.seconds || 0 }} seconds
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
  const birthDate = new Date(
    selectedDate.value + 'T' + selectedTime.value + ':00'
  );
  const currentDate = new Date();
  const ageInMilliseconds = currentDate - birthDate;

  const years = Math.floor(ageInMilliseconds / (365 * 24 * 60 * 60 * 1000));
  const remainingMilliseconds = ageInMilliseconds % (365 * 24 * 60 * 60 * 1000);

  const months = Math.floor(remainingMilliseconds / (30 * 24 * 60 * 60 * 1000));
  const remainingDaysMilliseconds =
    remainingMilliseconds % (30 * 24 * 60 * 60 * 1000);

  const days = Math.floor(remainingDaysMilliseconds / (24 * 60 * 60 * 1000));
  const remainingHoursMilliseconds =
    remainingDaysMilliseconds % (24 * 60 * 60 * 1000);

  const hours = Math.floor(remainingHoursMilliseconds / (60 * 60 * 1000));
  const remainingMinutesMilliseconds =
    remainingHoursMilliseconds % (60 * 60 * 1000);

  const minutes = Math.floor(remainingMinutesMilliseconds / (60 * 1000));
  const remainingSecondsMilliseconds =
    remainingMinutesMilliseconds % (60 * 1000);

  const seconds = Math.floor(remainingSecondsMilliseconds / 1000);

  return {
    years,
    months,
    days,
    hours,
    minutes,
    seconds
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
