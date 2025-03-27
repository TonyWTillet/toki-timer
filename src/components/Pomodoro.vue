<template>
    <section class="relative">
          <div class="relative z-10 max-w-screen-xl mx-auto px-4 py-28 md:px-8">
            <div class="space-y-5 max-w-4xl mx-auto text-center">
              <h2
                class="text-4xl text-white font-extrabold mx-auto md:text-5xl"
              >
                Pomodoro Timer for your productivity
              </h2>
              <p class="max-w-2xl mx-auto text-gray-400">
                A simple and effective way to manage your time and increase your productivity.
              </p>
              <div v-if="steps === 0" class="flex justify-center items-center gap-x-4 text-gray-400 text-sm ">
                <input
                  type="text"
                  placeholder="First Name"
                  v-model="firstName"
                  class="w-full px-3 py-2.5 text-center text-gray-400 bg-gray-700 focus:bg-gray-900 duration-150 outline-none rounded-lg shadow sm:max-w-sm sm:w-auto"
                />
                <button @click="nextStep" class="ml-2 w-full px-3 py-2.5 text-center text-white bg-indigo-600 focus:bg-indigo-700 duration-150 outline-none rounded-lg shadow sm:max-w-sm sm:w-auto flex items-center justify-center gap-x-2">
                    Start
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor" class="w-5 h-5">
                        <path fill-rule="evenodd" d="M5 10a.75.75 0 01.75-.75h6.638L10.23 7.29a.75.75 0 111.04-1.08l3.5 3.25a.75.75 0 010 1.08l-3.5 3.25a.75.75 0 11-1.04-1.08l2.158-1.96H5.75A.75.75 0 015 10z" clip-rule="evenodd" />
                    </svg>
                </button>
              </div>
                
              <div
                class="flex justify-center items-center gap-x-4 text-gray-400 text-sm"
              >
              </div>
            </div>
          </div>
          <div
            class="absolute inset-0 m-auto max-w-xs h-[357px] blur-[118px] sm:max-w-md md:max-w-lg"
            style="
              background: linear-gradient(
                106.89deg,
                rgba(192, 132, 252, 0.11) 15.73%,
                rgba(14, 165, 233, 0.41) 15.74%,
                rgba(232, 121, 249, 0.26) 56.49%,
                rgba(79, 70, 229, 0.4) 115.91%
              );
            "
          ></div>
        </section>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const time = ref(25);
const breakTime = ref(5);
const isBreak = ref(false);


const end = () => {
    isBreak.value = true;
}
</script>