<template>
    <AudioModalBase
        :modelValue="modelValue"
        id="sound-modal"
        title="Ambient Sound Settings"
        subtitle="Choose your ambient sounds"
        @volume-change="handleVolumeChange"
        @update:modelValue="$emit('update:modelValue', $event)"
    >
        <template #content>
            <!-- Sound Options -->
            <div 
                v-for="sound in ambientSounds" 
                :key="sound.id" 
                class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-colors cursor-pointer"
                :class="{ 'bg-indigo-500/20': currentSound?.id === sound.id }"
                @click="handleSoundClick(sound)"
            >
                <div class="flex items-center gap-4">
                    <div class="w-12 h-12 bg-indigo-500/20 rounded-lg flex items-center justify-center">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                        </svg>
                    </div>
                    <div class="flex-1">
                        <h3 class="text-white font-medium">{{ sound.name }}</h3>
                        <p class="text-white/60 text-sm">{{ currentSound?.id === sound.id ? (isPlaying ? 'Playing' : 'Paused') : 'Click to play' }}</p>
                    </div>
                    <button 
                        class="text-white/60 hover:text-white"
                        @click.stop="handleSoundClick(sound)"
                    >
                        <svg v-if="currentSound?.id === sound.id && isPlaying" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                        <svg v-else-if="currentSound?.id === sound.id && !isPlaying" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                        <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                    </button>
                </div>
            </div>
        </template>
    </AudioModalBase>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import AudioModalBase from './AudioModalBase.vue';

interface Sound {
    id: number;
    name: string;
    url: string;
}

const props = defineProps<{
    modelValue: boolean;
}>();

const emit = defineEmits<{
    (e: 'update:modelValue', value: boolean): void;
}>();

const ambientSounds = ref<Sound[]>([
    { id: 1, name: 'Rain', url: '/sounds/rain.mp3' },
    { id: 2, name: 'Ocean Waves', url: '/sounds/ocean.mp3' },
    { id: 3, name: 'Forest', url: '/sounds/forest.mp3' },
    { id: 4, name: 'White Noise', url: '/sounds/white-noise.mp3' },
    { id: 5, name: 'Fireplace', url: '/sounds/fireplace.mp3' },
]);

const currentSound = ref<Sound | null>(null);
const isPlaying = ref(false);
let audioPlayer: HTMLAudioElement | null = null;

// Handle sound click
const handleSoundClick = (sound: Sound) => {
    if (currentSound.value?.id === sound.id) {
        // Si c'est le même son, on bascule play/pause
        if (isPlaying.value) {
            audioPlayer?.pause();
            isPlaying.value = false;
        } else {
            audioPlayer?.play();
            isPlaying.value = true;
        }
    } else {
        // Si c'est un nouveau son, on le lance
        if (audioPlayer) {
            audioPlayer.pause();
        }
        
        currentSound.value = sound;
        audioPlayer = new Audio(sound.url);
        audioPlayer.loop = true;
        audioPlayer.volume = 0.5;
        audioPlayer.play();
        isPlaying.value = true;
    }
};

// Handle volume change
const handleVolumeChange = (newVolume: number) => {
    if (audioPlayer) {
        audioPlayer.volume = newVolume / 100;
    }
};
</script> 