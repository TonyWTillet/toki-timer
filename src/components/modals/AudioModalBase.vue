<template>
    <div 
        :id="id"
        class="fixed inset-y-0 right-0 w-[400px] bg-[#1a1a2e]/95 backdrop-blur-xl transform transition-transform duration-300 ease-in-out z-50 border-l border-white/10"
        :class="modelValue ? 'translate-x-0' : 'translate-x-full'"
    >
        <div class="h-full flex flex-col p-6">
            <!-- Header -->
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h2 class="text-xl font-semibold text-white">{{ title }}</h2>
                    <p class="text-sm text-white/60">{{ subtitle }}</p>
                </div>
                <button 
                    @click="closeModal"
                    class="text-white/60 hover:text-white transition-colors"
                >
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>

            <!-- Volume Control -->
            <div class="mb-6">
                <div class="flex items-center justify-between mb-2">
                    <span class="text-white/80 text-sm">Volume</span>
                    <span class="text-white/60 text-sm">{{ volume }}%</span>
                </div>
                <div class="relative">
                    <input 
                        type="range" 
                        :value="volume"
                        @input="updateVolume"
                        min="0" 
                        max="100" 
                        class="w-full h-2 bg-white/10 rounded-lg appearance-none cursor-pointer [&::-webkit-slider-thumb]:appearance-none [&::-webkit-slider-thumb]:w-4 [&::-webkit-slider-thumb]:h-4 [&::-webkit-slider-thumb]:rounded-full [&::-webkit-slider-thumb]:bg-white [&::-webkit-slider-thumb]:cursor-pointer [&::-webkit-slider-thumb]:border-2 [&::-webkit-slider-thumb]:border-indigo-500/20 [&::-webkit-slider-thumb]:shadow-lg [&::-webkit-slider-thumb]:transition-all [&::-webkit-slider-thumb]:hover:scale-110"
                        :style="{
                            background: `linear-gradient(to right, rgb(99, 102, 241) ${volume}%, rgba(255, 255, 255, 0.1) ${volume}%)`
                        }"
                    >
                </div>
            </div>

            <!-- Content -->
            <div class="flex-1 overflow-y-auto">
                <div class="space-y-4">
                    <slot name="content"></slot>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

interface Props {
    modelValue: boolean;
    id: string;
    title: string;
    subtitle: string;
}

const props = defineProps<Props>();

const emit = defineEmits<{
    (e: 'update:modelValue', value: boolean): void;
    (e: 'volume-change', value: number): void;
}>();

const volume = ref(30);

const closeModal = () => {
    emit('update:modelValue', false);
};

const updateVolume = (event: Event) => {
    const target = event.target as HTMLInputElement;
    volume.value = parseInt(target.value);
    emit('volume-change', volume.value);
};
</script> 