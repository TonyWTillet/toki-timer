<template>
    <div 
        id="background-modal"
        class="fixed inset-y-0 right-0 w-[400px] bg-[#1a1a2e]/95 backdrop-blur-xl transform transition-transform duration-300 ease-in-out z-50 border-l border-white/10"
        :class="modelValue ? 'translate-x-0' : 'translate-x-full'"
    >
        <div class="h-full flex flex-col p-6">
            <!-- Header -->
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h2 class="text-xl font-semibold text-white">Background Settings</h2>
                    <p class="text-sm text-white/60">Choose your preferred background</p>
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

            <!-- Tabs -->
            <div class="flex space-x-2 mb-6">
                <button 
                    v-for="tab in tabs" 
                    :key="tab.id"
                    @click="activeTab = tab.id"
                    class="px-4 py-2 rounded-lg text-sm font-medium transition-colors"
                    :class="activeTab === tab.id 
                        ? 'bg-indigo-500 text-white' 
                        : 'text-white/60 hover:text-white hover:bg-white/10'"
                >
                    {{ tab.name }}
                </button>
            </div>

            <!-- Content -->
            <div class="flex-1 overflow-y-auto">
                <!-- Static Images Section -->
                <div v-if="activeTab === 'static'" class="grid grid-cols-2 gap-4">
                    <div 
                        v-for="background in staticBackgrounds" 
                        :key="background.id" 
                        class="group relative aspect-video rounded-lg overflow-hidden cursor-pointer"
                        @click="selectBackground(background)"
                    >
                        <img 
                            :src="background.url" 
                            class="w-full h-full object-cover" 
                            :alt="background.name" 
                        />
                        <div 
                            class="absolute inset-0 bg-black/50 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity"
                        >
                            <span class="text-white text-sm font-medium">{{ background.name }}</span>
                        </div>
                        <!-- Selected Indicator -->
                        <div 
                            v-if="selectedBackground?.id === background.id"
                            class="absolute inset-0 ring-2 ring-indigo-500 rounded-lg"
                        >
                            <div class="absolute top-2 right-2 bg-indigo-500 rounded-full p-1">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white" viewBox="0 0 20 20" fill="currentColor">
                                    <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" />
                                </svg>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Animated Backgrounds Section -->
                <div v-if="activeTab === 'animated'" class="grid grid-cols-2 gap-4">
                    <div 
                        v-for="background in animatedBackgrounds" 
                        :key="background.id" 
                        class="group relative aspect-video rounded-lg overflow-hidden cursor-pointer"
                        @click="selectBackground(background)"
                    >
                        <div 
                            class="w-full h-full"
                            :class="background.animation"
                        ></div>
                        <div 
                            class="absolute inset-0 bg-black/50 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity"
                        >
                            <span class="text-white text-sm font-medium">{{ background.name }}</span>
                        </div>
                        <!-- Selected Indicator -->
                        <div 
                            v-if="selectedBackground?.id === background.id"
                            class="absolute inset-0 ring-2 ring-indigo-500 rounded-lg"
                        >
                            <div class="absolute top-2 right-2 bg-indigo-500 rounded-full p-1">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white" viewBox="0 0 20 20" fill="currentColor">
                                    <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" />
                                </svg>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Videos Section -->
                <div v-if="activeTab === 'videos'" class="grid grid-cols-2 gap-4">
                    <div 
                        v-for="background in videoBackgrounds" 
                        :key="background.id" 
                        class="group relative aspect-video rounded-lg overflow-hidden cursor-pointer"
                        @click="selectBackground(background)"
                    >
                        <video 
                            :src="background.url" 
                            class="w-full h-full object-cover"
                            loop
                            muted
                            autoplay
                        ></video>
                        <div 
                            class="absolute inset-0 bg-black/50 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity"
                        >
                            <span class="text-white text-sm font-medium">{{ background.name }}</span>
                        </div>
                        <!-- Selected Indicator -->
                        <div 
                            v-if="selectedBackground?.id === background.id"
                            class="absolute inset-0 ring-2 ring-indigo-500 rounded-lg"
                        >
                            <div class="absolute top-2 right-2 bg-indigo-500 rounded-full p-1">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-white" viewBox="0 0 20 20" fill="currentColor">
                                    <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" />
                                </svg>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';

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

const tabs = [
    { id: 'static', name: 'Static' },
    { id: 'animated', name: 'Animated' },
    { id: 'videos', name: 'Videos' }
];

const activeTab = ref('static');

const staticBackgrounds: StaticBackground[] = [
    { id: 1, name: 'Lofi Study', url: '/src/assets/img/backgrounds/lofi_study_pm.webp', type: 'static' },
    { id: 2, name: 'Lofi Jazz', url: '/src/assets/img/backgrounds/lofi_jazz.webp', type: 'static' },
    { id: 3, name: 'Lofi Parisan Romance', url: '/src/assets/img/backgrounds/lofi_parisian_romance.webp', type: 'static' },
    { id: 4, name: 'Sunset City', url: '/src/assets/img/backgrounds/sun_city.jpg', type: 'static' },
    { id: 5, name: 'Earth', url: '/src/assets/img/backgrounds/earth.jpg', type: 'static' },
    { id: 6, name: 'Japanese night', url: '/src/assets/img/backgrounds/japanese_night.png', type: 'static' },
    { id: 7, name: 'Lofi Buildings', url: '/src/assets/img/backgrounds/lofi_buildings.png', type: 'static' },
    { id: 8, name: 'Blue Car', url: '/src/assets/img/backgrounds/blue_car.jpg', type: 'static' },
    { id: 9, name: 'Forest', url: '/src/assets/img/backgrounds/forest.jpg', type: 'static' },
    { id: 10, name: 'Arcane', url: '/src/assets/img/backgrounds/arcane.png', type: 'static' },
    { id: 11, name: 'Cherry Blossom', url: '/src/assets/img/backgrounds/cherry_blossom.jpg', type: 'static' }
];

const animatedBackgrounds: AnimatedBackground[] = [
    { id: 7, name: 'Gradient Flow', animation: 'bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 animate-gradient', type: 'animated' },
    { id: 8, name: 'Soft Pulse', animation: 'bg-indigo-500/50 animate-pulse-slow', type: 'animated' },
    { id: 9, name: 'Rainbow', animation: 'bg-rainbow animate-rainbow', type: 'animated' },
    { id: 10, name: 'Wave', animation: 'bg-wave animate-wave', type: 'animated' },
    { id: 11, name: 'Ocean Blue', animation: 'bg-gradient-to-br from-blue-500 via-cyan-500 to-teal-500 animate-gradient', type: 'animated' },
    { id: 12, name: 'Sunset', animation: 'bg-gradient-to-r from-orange-500 via-red-500 to-pink-500 animate-gradient', type: 'animated' },
    { id: 13, name: 'Forest Green', animation: 'bg-gradient-to-br from-green-500 via-emerald-500 to-teal-500 animate-gradient', type: 'animated' },
    { id: 14, name: 'Purple Dream', animation: 'bg-gradient-to-r from-purple-500 via-fuchsia-500 to-pink-500 animate-gradient', type: 'animated' },
    { id: 15, name: 'Aurora', animation: 'bg-gradient-to-r from-green-400 via-blue-500 to-purple-500 animate-gradient', type: 'animated' },
    { id: 16, name: 'Neon', animation: 'bg-gradient-to-r from-cyan-400 via-blue-500 to-purple-500 animate-gradient', type: 'animated' }
];

const videoBackgrounds: VideoBackground[] = [
    { id: 13, name: 'Rain', url: '/src/assets/videos/rain.mp4', type: 'video' },
    { id: 14, name: 'Ocean Waves', url: '/src/assets/videos/ocean.mp4', type: 'video' },
    { id: 15, name: 'Forest Stream', url: '/src/assets/videos/forest_stream.mp4', type: 'video' },
    { id: 16, name: 'Fireplace', url: '/src/assets/videos/fireplace.mp4', type: 'video' },
    { id: 17, name: 'Coffee Shop', url: '/src/assets/videos/coffee_shop.mp4', type: 'video' },
    { id: 18, name: 'City Rain', url: '/src/assets/videos/city_rain.mp4', type: 'video' }
];

const selectedBackground = ref<Background | null>(staticBackgrounds[0]);

// Au premier rendu, le background est le premier background statique
onMounted(() => {
    selectedBackground.value = staticBackgrounds[0];
    emit('select-background', selectedBackground.value);
});

const props = defineProps<{
    modelValue: boolean
}>();

const emit = defineEmits<{
    (e: 'update:modelValue', value: boolean): void
    (e: 'select-background', background: Background): void
}>();

const selectBackground = (background: Background) => {
    selectedBackground.value = background;
    emit('select-background', background);
};
</script>

<style>
@keyframes gradient {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
}

@keyframes rainbow {
    0% { background-color: #ff0000; }
    20% { background-color: #ff00ff; }
    40% { background-color: #0000ff; }
    60% { background-color: #00ffff; }
    80% { background-color: #00ff00; }
    100% { background-color: #ff0000; }
}

@keyframes twinkle {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.3; }
}

@keyframes wave {
    0% { transform: translateX(0); }
    50% { transform: translateX(-25%); }
    100% { transform: translateX(0); }
}

@keyframes float {
    0% { transform: translateY(0px); }
    50% { transform: translateY(-20px); }
    100% { transform: translateY(0px); }
}

.animate-gradient {
    background-size: 200% 200%;
    animation: gradient 15s ease infinite;
}

.animate-rainbow {
    animation: rainbow 10s linear infinite;
}

.animate-twinkle {
    animation: twinkle 2s ease-in-out infinite;
}

.animate-wave {
    animation: wave 10s linear infinite;
}

.animate-particles {
    animation: float 6s ease-in-out infinite;
}

.bg-rainbow {
    background: linear-gradient(to right, #ff0000, #ff00ff, #0000ff, #00ffff, #00ff00);
}

.bg-stars {
    background-color: #1a1a2e;
    position: relative;
    overflow: hidden;
}

.bg-stars::before,
.bg-stars::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-image: 
        radial-gradient(2px 2px at 20px 30px, #fff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 40px 70px, #fff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 50px 160px, #fff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 90px 40px, #fff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 130px 80px, #fff, rgba(0,0,0,0));
    background-repeat: repeat;
    animation: twinkle 4s ease-in-out infinite;
}

.bg-stars::after {
    background-image: 
        radial-gradient(2px 2px at 150px 150px, #fff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 200px 200px, #fff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 250px 250px, #fff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 300px 300px, #fff, rgba(0,0,0,0)),
        radial-gradient(2px 2px at 350px 350px, #fff, rgba(0,0,0,0));
    animation-delay: 2s;
}

.bg-wave {
    background: linear-gradient(45deg, #4f46e5, #7c3aed, #2563eb);
    background-size: 400% 400%;
}

.bg-particles {
    background-color: #1a1a2e;
    position: relative;
    overflow: hidden;
}

.bg-particles::before,
.bg-particles::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-image: 
        radial-gradient(1px 1px at 10px 10px, #fff, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 20px 20px, #fff, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 30px 30px, #fff, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 40px 40px, #fff, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 50px 50px, #fff, rgba(0,0,0,0));
    background-repeat: repeat;
    animation: float 6s ease-in-out infinite;
}

.bg-particles::after {
    background-image: 
        radial-gradient(1px 1px at 60px 60px, #fff, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 70px 70px, #fff, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 80px 80px, #fff, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 90px 90px, #fff, rgba(0,0,0,0)),
        radial-gradient(1px 1px at 100px 100px, #fff, rgba(0,0,0,0));
    animation-delay: 3s;
}

.animate-pulse-slow {
    animation: pulse 4s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

@keyframes pulse {
    0%, 100% {
        opacity: 0.5;
    }
    50% {
        opacity: 0.8;
    }
}
</style> 