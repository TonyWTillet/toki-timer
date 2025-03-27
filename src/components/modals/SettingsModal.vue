<template>
    <div 
        id="settings-modal"
        class="fixed inset-y-0 right-0 w-[400px] bg-[#1a1a2e]/95 backdrop-blur-xl transform transition-transform duration-300 ease-in-out z-50 border-l border-white/10"
        :class="modelValue ? 'translate-x-0' : 'translate-x-full'"
    >
        <div class="h-full flex flex-col p-6">
            <!-- Header -->
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h2 class="text-xl font-semibold text-white">Timer Settings</h2>
                    <p class="text-sm text-white/60">Customize your timer preferences</p>
                </div>
                <button 
                    @click="$emit('update:modelValue', false)"
                    class="text-white/60 hover:text-white transition-colors"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>

            <!-- Content -->
            <div class="flex-1 overflow-y-auto">
                <div class="space-y-6">
                    <!-- Focus Duration -->
                    <div class="space-y-2">
                        <label class="text-white text-sm font-medium">Focus Duration</label>
                        <input 
                            type="range" 
                            min="15" 
                            max="60" 
                            step="5"
                            v-model="focusDuration"
                            class="w-full"
                        />
                        <div class="flex justify-between text-white/60 text-sm">
                            <span>{{ focusDuration }} minutes</span>
                            <button class="hover:text-white" @click="focusDuration = 25">Reset</button>
                        </div>
                    </div>

                    <!-- Break Duration -->
                    <div class="space-y-2">
                        <label class="text-white text-sm font-medium">Break Duration</label>
                        <input 
                            type="range" 
                            min="5" 
                            max="30" 
                            step="5"
                            v-model="breakDuration"
                            class="w-full"
                        />
                        <div class="flex justify-between text-white/60 text-sm">
                            <span>{{ breakDuration }} minutes</span>
                            <button class="hover:text-white" @click="breakDuration = 5">Reset</button>
                        </div>
                    </div>

                    <!-- Sessions -->
                    <div class="space-y-2">
                        <label class="text-white text-sm font-medium">Sessions Before Long Break</label>
                        <div class="flex gap-2">
                            <button 
                                v-for="n in 4" 
                                :key="n"
                                @click="sessions = n"
                                :class="[
                                    'flex-1 py-2 rounded-lg text-sm font-medium transition-colors',
                                    sessions === n 
                                        ? 'bg-indigo-600 text-white' 
                                        : 'bg-white/5 text-white/60 hover:bg-white/10'
                                ]"
                            >
                                {{ n }}
                            </button>
                        </div>
                    </div>

                    <!-- Auto Start Breaks -->
                    <div class="flex items-center justify-between">
                        <label class="text-white text-sm font-medium">Auto-start Breaks</label>
                        <button 
                            @click="autoStartBreaks = !autoStartBreaks"
                            :class="[
                                'w-11 h-6 rounded-full transition-colors',
                                autoStartBreaks ? 'bg-indigo-600' : 'bg-white/5'
                            ]"
                        >
                            <div 
                                :class="[
                                    'w-5 h-5 rounded-full bg-white transform transition-transform',
                                    autoStartBreaks ? 'translate-x-6' : 'translate-x-0.5'
                                ]"
                            ></div>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

defineProps<{
    modelValue: boolean
}>();

defineEmits<{
    (e: 'update:modelValue', value: boolean): void
}>();

const focusDuration = ref(25);
const breakDuration = ref(5);
const sessions = ref(4);
const autoStartBreaks = ref(false);
</script> 