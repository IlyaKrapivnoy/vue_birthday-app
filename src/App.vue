<template>
  <div
    class="h-screen bg-gradient-to-r from-blue-500 to-green-500 flex flex-col justify-center items-center text-center"
  >
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
    <h1 class="text-[140px] font-bold uppercase leading-[110px]">
      Birthday App
    </h1>
    <div class="w-4/6">
      <div class="flex justify-between">
        <div class="text-gray-900">
          You have lived:
          <span>{{ livedMilliseconds || 0 }}</span>
          milliseconds
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue';

const selectedDate = ref(localStorage.getItem('selectedDate') || '');
const selectedTime = ref(localStorage.getItem('selectedTime') || '');

const handleDateChange = () => {
  localStorage.setItem('selectedDate', selectedDate.value);
};

const handleTimeChange = () => {
  localStorage.setItem('selectedTime', selectedTime.value);
};

const livedMilliseconds = ref(0);

const updateAge = () => {
  const timerInterval = setInterval(() => {
    const birthDate = new Date(
      selectedDate.value + 'T' + selectedTime.value + ':00'
    );
    const currentDate = new Date();
    livedMilliseconds.value = currentDate - birthDate;
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
