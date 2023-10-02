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
          Time until your next birthday (days):
          <div
            class="text-gray-800 font-bold"
            v-if="age.months === 0 && age.days === 0"
          >
            Happy B-Day!
          </div>
          <div v-else>
            <span id="countdownDays">{{ countdownDays }}</span> days
          </div>
        </div>

        <div class="text-gray-300">
          Time until your next birthday (milliseconds):
          <div
            class="text-gray-800 font-bold"
            v-if="age.months === 0 && age.days === 0"
          >
            Happy B-Day!
          </div>
          <div v-else>
            <span id="countdownMilliseconds">{{ countdownMilliseconds }}</span>
            milliseconds
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref, watch } from 'vue';

const selectedDate = ref(localStorage.getItem('selectedDate') || '');
const selectedTime = ref(localStorage.getItem('selectedTime') || '');

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

const countdownMilliseconds = computed(() => {
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

  return nextBirthday - today;
});

watch(age, () => {
  countdownMilliseconds.value = 0;
});

const getCurrentDate = () => {
  const currentDate = new Date();
  const year = currentDate.getFullYear();
  const month = String(currentDate.getMonth() + 1).padStart(2, '0');
  const day = String(currentDate.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
};

const countdownDays = computed(() => {
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
  return Math.floor(timeDifference / (1000 * 60 * 60 * 24));
});
</script>
