<template>
    <div>
      <div class="bg-gray-800 h-screen w-screen flex flex-col justify-center items-center">
        <Header />
        
        <div class="h-screen w-screen flex flex-col justify-center items-center" v-if="loginForm">
          <UserSteps />
        </div>

        <div class="h-screen w-screen flex flex-col justify-center items-center" v-if="!loginForm">
          <Pomodoro />
        </div>
       
      </div>
    </div>
  </template>
  
  <script setup lang="ts">
    import { ref } from 'vue';
    import Header from './layouts/Header.vue';
    import UserSteps from './UserSteps.vue';
    import Pomodoro from './Pomodoro.vue';

    const loginForm = ref(true);


  </script>