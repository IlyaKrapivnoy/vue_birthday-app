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
      {{ age.years.value || 0 }} years, {{ age.months.value || 0 }} months,
      {{ age.days.value || 0 }} days, {{ age.hours.value || 0 }} hours,
      {{ age.minutes.value || 0 }} minutes, {{ age.seconds.value || 0 }} seconds
    </p>
  </div>
</template>

<script setup>
import { computed, ref, onMounted, onBeforeUnmount } from 'vue';

const selectedDate = ref('');
const selectedTime = ref('');

const handleDateChange = () => {
  console.log('Selected Date:', selectedDate.value);
};

const handleTimeChange = () => {
  console.log('Selected Time:', selectedTime.value);
};

const age = {
  years: ref(0),
  months: ref(0),
  days: ref(0),
  hours: ref(0),
  minutes: ref(0),
  seconds: ref(0)
};

const updateAge = () => {
  const timerInterval = setInterval(() => {
    const birthDate = new Date(
      selectedDate.value + 'T' + selectedTime.value + ':00'
    );
    const currentDate = new Date();
    const ageInMilliseconds = currentDate - birthDate;

    age.years.value = Math.floor(
      ageInMilliseconds / (365 * 24 * 60 * 60 * 1000)
    );
    const remainingMilliseconds =
      ageInMilliseconds % (365 * 24 * 60 * 60 * 1000);

    age.months.value = Math.floor(
      remainingMilliseconds / (30 * 24 * 60 * 60 * 1000)
    );
    const remainingDaysMilliseconds =
      remainingMilliseconds % (30 * 24 * 60 * 60 * 1000);

    age.days.value = Math.floor(
      remainingDaysMilliseconds / (24 * 60 * 60 * 1000)
    );
    const remainingHoursMilliseconds =
      remainingDaysMilliseconds % (24 * 60 * 60 * 1000);

    age.hours.value = Math.floor(remainingHoursMilliseconds / (60 * 60 * 1000));
    const remainingMinutesMilliseconds =
      remainingHoursMilliseconds % (60 * 60 * 1000);

    age.minutes.value = Math.floor(remainingMinutesMilliseconds / (60 * 1000));
    const remainingSecondsMilliseconds =
      remainingMinutesMilliseconds % (60 * 1000);

    age.seconds.value = Math.floor(remainingSecondsMilliseconds / 1000);
  }, 1000);

  onBeforeUnmount(() => {
    clearInterval(timerInterval);
  });
};

onMounted(() => {
  updateAge();
});

const getCurrentDate = () => {
  const currentDate = new Date();
  const year = currentDate.getFullYear();
  const month = String(currentDate.getMonth() + 1).padStart(2, '0');
  const day = String(currentDate.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
};
</script>
