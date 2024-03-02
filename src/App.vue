<script setup lang="ts">
import {ref} from "vue";

const dateUid = ref(Date.now().toString(36));
const isButtonDisabled = ref(false);
const isMessage = ref(false);

const dateStringUid = ref("lt9hemhc");

const handleGenerateDateUid = () => {
  dateUid.value = Date.now().toString(36);
  isButtonDisabled.value = true;
  setTimeout(() => {
    isButtonDisabled.value = false;
  }, 1);
};

const copyText = async () => {
  try {
    await navigator.clipboard.writeText(dateUid.value);
    isMessage.value = true;
  } catch (err) {
    console.error("Failed to copy: ", err);
  }
  setTimeout(() => {
    isMessage.value = false;
  }, 600);
};
</script>

<template>
  <Transition name="slide">
    <div
      v-show="isMessage"
      class="message px-4 py-2 rounded-md right-3 fixed top-3 bg-green-500 text-white min-w-56">
      Copied
    </div>
  </Transition>
  <section class="bg-gray-100 h-screen flex items-center justify-center gap-4">
    <div class="bg-white rounded-lg shadow p-6 space-y-4">
      <p>Your Unique Generated Date ID</p>
      <h3
        @click="copyText"
        class="font-bold text-3xl text-center cursor-pointer">
        {{ dateUid }}
      </h3>
      <button
        @click="handleGenerateDateUid"
        :disabled="isButtonDisabled"
        type="button"
        class="w-full text-white bg-gray-700 hover:bg-gray-800 focus:ring-4 focus:ring-gray-300 font-medium rounded-lg text-sm px-5 py-2 dark:bg-gray-600 dark:hover:bg-gray-700 focus:outline-none dark:focus:ring-gray-800 disabled:bg-opacity-50 disabled:cursor-wait">
        Generate
      </button>
    </div>
    <div class="bg-white rounded-lg shadow p-6 space-y-3">
      <p>Convert Date ID to Date</p>
      <input
        type="text"
        id="first_name"
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        placeholder="lt9hemhc"
        v-model="dateStringUid"
        required />
      <h3 class="font-medium text-center">
        {{ new Date(parseInt(dateStringUid, 36)).toLocaleTimeString() }}
        {{ new Date(parseInt(dateStringUid, 36)).toLocaleDateString() }}
      </h3>
    </div>
  </section>
</template>

<style>
.slide-enter-active,
.slide-leave-active {
  transition: opacity 0.5s ease;
}

.slide-enter-from,
.slide-leave-to {
  opacity: 0.5;
}
</style>
