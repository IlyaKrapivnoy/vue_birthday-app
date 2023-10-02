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
          Time until your next birthday:
          <span class="text-gray-800 font-bold">{{
            timeUntilNextBirthday
          }}</span>
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
import { ref, onMounted, onBeforeUnmount, computed } from 'vue';

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

    const ageDate = new Date(ageInMilliseconds);

    const years = ageDate.getUTCFullYear() - 1970;
    const months = ageDate.getUTCMonth();
    const days = ageDate.getUTCDate() - 1;

    const hours = currentDate.getHours();
    const minutes = currentDate.getMinutes();
    const seconds = currentDate.getSeconds();

    age.years.value = years;
    age.months.value = months;
    age.days.value = days;
    age.hours.value = hours;
    age.minutes.value = minutes;
    age.seconds.value = seconds;

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

const timeUntilNextBirthday = computed(() => {
  if (age.months.value === 0 && age.days.value === 0) {
    return 'Happy B-Day!';
  }

  const today = new Date();
  const selectedDateTime = new Date(
    selectedDate.value + 'T' + selectedTime.value + ':00'
  );
  const nextBirthday = new Date(
    today.getFullYear(),
    selectedDateTime.getMonth(),
    selectedDateTime.getDate()
  );

  if (nextBirthday < today) {
    nextBirthday.setFullYear(nextBirthday.getFullYear() + 1);
  }

  const timeDifference = nextBirthday - today;
  const days = Math.floor(timeDifference / (1000 * 60 * 60 * 24));
  const hours = Math.floor(
    (timeDifference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60)
  );
  const minutes = Math.floor((timeDifference % (1000 * 60 * 60)) / (1000 * 60));
  const seconds = Math.floor((timeDifference % (1000 * 60)) / 1000);

  return `${days} days, ${hours} hours, ${minutes} minutes, ${seconds} seconds`;
});
</script>
