<template>
  <div
    class="h-screen bg-gradient-to-r from-blue-500 to-green-500 flex flex-col justify-center items-center text-center"
  >
    <h1 class="text-[140px] font-bold uppercase">Birthday App</h1>

    <div class="w-4/6">
      <div class="flex justify-between -mt-10">
        <div class="flex justify-center gap-2">
          <input
            id="dateInput"
            type="date"
            v-model="selectedDate"
            @change="handleDateChange"
            :max="getCurrentDate()"
          />
          <input
            id="timeInput"
            type="time"
            v-model="selectedTime"
            @input="handleTimeChange"
          />
        </div>

        <div class="text-gray-300">
          Time for the next B-Day left:
          <span class="text-gray-800 font-bold">xxx</span>
        </div>
      </div>

      <div class="mt-20">
        <h2 class="text-gray-300">Your Current Age:</h2>
        <p v-if="showAgeCalculation" class="text-3xl text-white">
          Calculating<span class="animate-ping">...</span>
        </p>
        <p v-else class="text-3xl text-white">
          {{ age.years }} years, {{ age.months }} months, {{ age.days }} days,
          {{ age.hours }} hours, {{ age.minutes }} minutes,
          {{ age.seconds }} seconds
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

const selectedDate = ref(localStorage.getItem('selectedDate') || '');
const selectedTime = ref(localStorage.getItem('selectedTime') || '');
const showAgeCalculation = ref(true);

const handleDateChange = () => {
  localStorage.setItem('selectedDate', selectedDate.value);
  console.log('Selected Date:', selectedDate.value);
};

const handleTimeChange = () => {
  localStorage.setItem('selectedTime', selectedTime.value);
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

    showAgeCalculation.value = false;
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
