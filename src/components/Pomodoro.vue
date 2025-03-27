<template>
    <div class="relative h-screen w-screen bg-[#1a1a2e] flex flex-col items-center justify-center">
        <!-- Current Time Display -->
        <div class="absolute top-4 right-4 z-10">
            <CurrentTime />
        </div>

        <!-- Logo and Quote Section -->
        <div class="absolute top-8 w-full px-8 flex justify-between items-center z-10">
            <div class="text-white text-2xl font-bold">TokiTimer</div>
            <div class="text-white text-sm italic">
              {{ currentTime }}
            </div>
        </div>

        <!-- Main Timer Section with Card Design -->
        <div class="text-center z-10">
            <div class="bg-white/10 backdrop-blur-sm bg-opacity-0 rounded-2xl p-12 shadow-2xl border border-white/20">
                <div class="text-white mb-8">
                    <h2 class="text-2xl mb-2 font-semibold">Thursday's all yours, tony.</h2>
                    <p class="text-xl opacity-90">Keep going!</p>
                </div>
                
                <!-- Timer Display -->
                <div class="bg-white/5 rounded-xl p-8 mb-6">
                    <div class="text-white text-[150px] font-bold tracking-wider leading-none">
                        25:00
                    </div>
                </div>

                <!-- Timer Controls -->
                <div class="flex justify-center gap-6">
                  <button class="w-full px-4 text-center text-white bg-indigo-600 hover:bg-indigo-500 focus:bg-indigo-800 duration-150 outline-none rounded-full shadow sm:max-w-sm sm:w-auto flex items-center justify-center">
                    Start
                  </button>
                
                    
                    <button class="bg-white/20 hover:bg-white/30 transition-all duration-300 rounded-full p-4 flex items-center justify-center text-white">
                      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M16.023 9.348h4.992v-.001M2.985 19.644v-4.992m0 0h4.992m-4.993 0 3.181 3.183a8.25 8.25 0 0 0 13.803-3.7M4.031 9.865a8.25 8.25 0 0 1 13.803-3.7l3.181 3.182m0-4.991v4.99" />
                      </svg>

                    </button>
                    
                    <button class="bg-white/20 hover:bg-white/30 transition-all duration-300 rounded-full p-4 text-white">
                      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="size-6">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M9 9V4.5M9 9H4.5M9 9 3.75 3.75M9 15v4.5M9 15H4.5M9 15l-5.25 5.25M15 9h4.5M15 9V4.5M15 9l5.25-5.25M15 15h4.5M15 15v4.5m0-4.5 5.25 5.25" />
                    </svg>

                    </button>
                </div>

                <!-- Session Info -->
                <div class="mt-8 flex justify-between items-center text-white/70 text-sm">
                    <div>Session 1/4</div>
                    <div>Focus Time: 25:00 - Break Time: 5:00</div>
                </div>
            </div>

            <!-- Progress Bar -->
            <div class="mt-6 w-full bg-white/10 rounded-full h-1.5">
                <div class="bg-white w-1/2 h-full rounded-full"></div>
            </div>
        </div>

        <!-- Bottom Controls -->
        <div class="absolute bottom-8 w-full px-8 z-10">
            <div class="flex justify-end items-center">
                <!-- Center Controls -->
                <div class="flex gap-4">

                    <!-- Background Settings -->
                    <button class="text-white hover:text-white/80 transition-all duration-300" @click="showBackgroundModal = true">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
                        </svg>
                    </button>

                    <!-- Music Settings -->
                    <button class="text-white hover:text-white/80 transition-all duration-300" @click="showMusicModal = true">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zm12-3c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2zM9 10l12-3" />
                        </svg>
                    </button>

                    <!-- Ambient Sound Settings -->
                    <button class="text-white hover:text-white/80 transition-all duration-300" @click="showSoundModal = true">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                        </svg>
                    </button>

                    <!-- Settings -->
                    <button class="text-white hover:text-white/80 transition-all duration-300" @click="showSettingsModal = true">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                        </svg>
                    </button>
                </div>
            </div>
        </div>

        <!-- Social Links -->
        <div class="absolute bottom-8 left-8 z-10 flex gap-4">
            <a 
                href="https://www.linkedin.com/in/tonytilletdevweb/" 
                target="_blank"
                class="text-white hover:text-white/80 transition-all duration-300"
            >
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
                    <path d="M19 0h-14c-2.761 0-5 2.239-5 5v14c0 2.761 2.239 5 5 5h14c2.762 0 5-2.239 5-5v-14c0-2.761-2.238-5-5-5zm-11 19h-3v-11h3v11zm-1.5-12.268c-.966 0-1.75-.79-1.75-1.764s.784-1.764 1.75-1.764 1.75.79 1.75 1.764-.783 1.764-1.75 1.764zm13.5 12.268h-3v-5.604c0-3.368-4-3.113-4 0v5.604h-3v-11h3v1.765c1.396-2.586 7-2.777 7 2.476v6.759z"/>
                </svg>
            </a>
            <a 
                href="https://github.com/TonyWTillet" 
                target="_blank"
                class="text-white hover:text-white/80 transition-all duration-300"
            >
                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
                    <path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/>
                </svg>
            </a>
        </div>

        <!-- Background Lofi Study Image with Overlay -->
        <div class="absolute inset-0 z-0">
            <!-- Static or Video Background -->
            <div 
                v-if="selectedBackground?.type === 'static' || selectedBackground?.type === 'video'"
                class="absolute inset-0 bg-cover bg-center bg-no-repeat transition-all duration-500"
                :style="{ backgroundImage: selectedBackground?.type === 'static' ? `url(${selectedBackground.url})` : 'none' }"
            >
                <!-- Video Background -->
                <video 
                    v-if="selectedBackground?.type === 'video'"
                    :src="selectedBackground.url"
                    class="w-full h-full object-cover"
                    loop
                    muted
                    autoplay
                ></video>
            </div>
            <!-- Animated Background -->
            <div 
                v-else-if="selectedBackground?.type === 'animated'"
                class="absolute inset-0 transition-all duration-500"
                :class="selectedBackground.animation"
            ></div>
            <!-- Default Background -->
            <div 
                v-else
                class="absolute inset-0 bg-cover bg-center bg-no-repeat transition-all duration-500"
                style="background-image: url('/src/assets/img/backgrounds/lofi_study.jpg')"
            ></div>
            <div class="absolute inset-0 bg-[#1a1a2e]/40"></div>
        </div>

        <!-- Background Modal -->
        <BackgroundModal 
            v-model="showBackgroundModal" 
            @select-background="handleBackgroundChange"
        />

        <!-- Music Modal -->
        <MusicModal v-model="showMusicModal" />

        <!-- Sound Modal -->
        <SoundModal v-model="showSoundModal" />

        <!-- Settings Modal -->
        <SettingsModal v-model="showSettingsModal" />
    </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, onUnmounted, computed } from 'vue';
import BackgroundModal from './modals/BackgroundModal.vue';
import MusicModal from './modals/MusicModal.vue';
import SoundModal from './modals/SoundModal.vue';
import SettingsModal from './modals/SettingsModal.vue';
import CurrentTime from './CurrentTime.vue';


// Get current time : Hours and Minutes

interface StaticBackground {
    id: number;
    name: string;
    url: string;
    type: 'static';
}

interface AnimatedBackground {
    id: number;
    name: string;
    animation: string;
    type: 'animated';
}

interface VideoBackground {
    id: number;
    name: string;
    url: string;
    type: 'video';
}

type Background = StaticBackground | AnimatedBackground | VideoBackground;

const showBackgroundModal = ref(false);
const showMusicModal = ref(false);
const showSoundModal = ref(false);
const showSettingsModal = ref(false);

const time = ref(25);
const breakTime = ref(5);

const isBreak = ref(false);


const selectedBackground = ref<Background | null>(null);

// Watch each modal state and close others when one opens
watch(showBackgroundModal, (newValue) => {
    if (newValue) {
        showMusicModal.value = false;
        showSoundModal.value = false;
        showSettingsModal.value = false;
    }
});

watch(showMusicModal, (newValue) => {
    if (newValue) {
        showBackgroundModal.value = false;
        showSoundModal.value = false;
        showSettingsModal.value = false;
    }
});

watch(showSoundModal, (newValue) => {
    if (newValue) {
        showBackgroundModal.value = false;
        showMusicModal.value = false;
        showSettingsModal.value = false;
    }
});

watch(showSettingsModal, (newValue) => {
    if (newValue) {
        showBackgroundModal.value = false;
        showMusicModal.value = false;
        showSoundModal.value = false;
    }
});

// Add click outside handler to close modals
const handleClickOutside = (event: MouseEvent) => {
    const target = event.target as HTMLElement;
    // Check if click is outside all modals
    const isOutsideBackground = !target.closest('#background-modal');
    const isOutsideMusic = !target.closest('#music-modal');
    const isOutsideSound = !target.closest('#sound-modal'); 
    const isOutsideSettings = !target.closest('#settings-modal');

    if (isOutsideBackground && isOutsideMusic && isOutsideSound && isOutsideSettings) {
        showBackgroundModal.value = false;
        showMusicModal.value = false;
        showSoundModal.value = false;
        showSettingsModal.value = false;
    }
};

// Add escape key handler
const handleEscapeKey = (event: KeyboardEvent) => {
    if (event.key === 'Escape') {
        showBackgroundModal.value = false;
        showMusicModal.value = false;
        showSoundModal.value = false;
        showSettingsModal.value = false;
    }
};

onMounted(() => {
    document.addEventListener('mousedown', handleClickOutside);
    document.addEventListener('keydown', handleEscapeKey);
});

onUnmounted(() => {
    document.removeEventListener('mousedown', handleClickOutside);
    document.removeEventListener('keydown', handleEscapeKey);
});

const handleBackgroundChange = (background: Background) => {
    selectedBackground.value = background;
};
</script>