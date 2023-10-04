<template>
  <div
    class="h-screen bg-black flex flex-col justify-center items-center text-center"
  >
    <div class="flex w-2/4 justify-between">
      <label for="birthdate" class="text-white">Enter your birthdate:</label>
      <div>
        <input
          id="birthdate"
          type="date"
          v-model="birthdate"
          :max="getCurrentDate()"
        />
        <input type="time" v-model="birthtime" />
        <button @click="resetForm" class="text-white ml-2">Reset</button>
      </div>
    </div>

    <div class="text-white">
      <h1 class="text-[120px]">You have lived:</h1>

      <h3 class="text-[70px] text-yellow-100">
        {{ formattedLivedMillisecondsMessage }}
      </h3>

      <h3 v-if="showMessage"></h3>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount, watch } from 'vue';

const birthdate = ref(localStorage.getItem('birthdate') || '');
const birthtime = ref(localStorage.getItem('birthtime') || '');

const getCurrentDate = () => {
  const currentDate = new Date();
  const year = currentDate.getFullYear();
  const month = String(currentDate.getMonth() + 1).padStart(2, '0');
  const day = String(currentDate.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
};

const birthDateTime = computed(() => {
  return new Date(`${birthdate.value}T${birthtime.value}:00`).getTime();
});

const livedMilliseconds = ref(0);

const showMessage = computed(() => {
  return !birthdate.value || !birthtime.value;
});

const livedMillisecondsMessage = computed(() => {
  return showMessage.value
    ? 'Select date and time of your B-Day'
    : livedMilliseconds.value.toString();
});

const formattedLivedMillisecondsMessage = computed(() => {
  if (showMessage.value) return livedMillisecondsMessage.value;

  const message = livedMillisecondsMessage.value;
  const parts = [];
  for (let i = 0; i < message.length; i += 3) {
    parts.push(message.slice(i, i + 3));
  }

  return parts.join('.');
});

const updateLivedMilliseconds = () => {
  const currentDateTime = new Date().getTime();
  livedMilliseconds.value = showMessage.value
    ? 0
    : currentDateTime - birthDateTime.value;
};

const resetForm = () => {
  birthdate.value = '';
  birthtime.value = '';
  livedMilliseconds.value = 0;
};

watch([birthdate, birthtime], () => {
  localStorage.setItem('birthdate', birthdate.value);
  localStorage.setItem('birthtime', birthtime.value);
});

let timerInterval;

onMounted(() => {
  updateLivedMilliseconds();
  timerInterval = setInterval(updateLivedMilliseconds, 1);
});

onBeforeUnmount(() => {
  clearInterval(timerInterval);
});
</script>
