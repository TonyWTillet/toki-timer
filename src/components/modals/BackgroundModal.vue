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
            <div class="flex space-x-2 mb-6 overflow-x-auto scrollbar-hide">
                <button 
                    v-for="tab in tabs" 
                    :key="tab.id"
                    @click="activeTab = tab.id"
                    class="px-4 py-2 rounded-lg text-sm font-medium transition-colors whitespace-nowrap"
                    :class="activeTab === tab.id 
                        ? 'bg-indigo-500 text-white' 
                        : 'text-white/60 hover:text-white hover:bg-white/10'"
                >
                    {{ tab.name }}
                </button>
            </div>

            <!-- Content -->
            <div class="flex-1 overflow-y-auto custom-scrollbar">
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
                            class="w-full h-full absolute inset-0"
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
    { id: 'animated', name: 'Colors' },
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
    { id: 1, name: 'Rose & Peach', animation: 'bg-cozy-sunset', type: 'animated' },
    { id: 2, name: 'Ocean Blue', animation: 'bg-ocean-waves', type: 'animated' },
    { id: 3, name: 'Lavender & Blue', animation: 'bg-lavender-mist', type: 'animated' },
    { id: 4, name: 'Forest Green', animation: 'bg-forest-dawn', type: 'animated' },
    { id: 5, name: 'Deep Navy', animation: 'bg-deep-navy', type: 'animated' },
    { id: 6, name: 'Slate Gray', animation: 'bg-slate-gray', type: 'animated' },
    { id: 7, name: 'Ocean Deep', animation: 'bg-ocean-deep', type: 'animated' },
    { id: 8, name: 'Golden Orange', animation: 'bg-autumn-leaves', type: 'animated' },
    { id: 9, name: 'Purple Dream', animation: 'bg-purple-dream', type: 'animated' },
    { id: 10, name: 'Mint Fresh', animation: 'bg-mint-fresh', type: 'animated' },
    { id: 11, name: 'Sunset Pink', animation: 'bg-sunset-pink', type: 'animated' },
    { id: 12, name: 'Midnight Blue', animation: 'bg-midnight-blue', type: 'animated' },
    { id: 13, name: 'Soft Coral', animation: 'bg-soft-coral', type: 'animated' },
    { id: 14, name: 'Sage Green', animation: 'bg-sage-green', type: 'animated' },
    { id: 15, name: 'Dusty Rose', animation: 'bg-dusty-rose', type: 'animated' },
    { id: 16, name: 'Ocean Breeze', animation: 'bg-ocean-breeze', type: 'animated' },
    { id: 17, name: 'Lavender Mist', animation: 'bg-lavender-mist-2', type: 'animated' },
    { id: 18, name: 'Warm Sand', animation: 'bg-warm-sand', type: 'animated' },
    { id: 19, name: 'Cool Mint', animation: 'bg-cool-mint', type: 'animated' },
    { id: 20, name: 'Dusty Blue', animation: 'bg-dusty-blue', type: 'animated' },
    { id: 21, name: 'Soft Peach', animation: 'bg-soft-peach', type: 'animated' },
    { id: 22, name: 'Forest Mist', animation: 'bg-forest-mist', type: 'animated' },
    { id: 23, name: 'Deep Purple', animation: 'bg-deep-purple', type: 'animated' },
    { id: 24, name: 'Dark Forest', animation: 'bg-dark-forest', type: 'animated' },
    { id: 25, name: 'Midnight Indigo', animation: 'bg-midnight-indigo', type: 'animated' },
    { id: 26, name: 'Deep Ocean', animation: 'bg-deep-ocean-2', type: 'animated' },
    { id: 27, name: 'Dark Slate', animation: 'bg-dark-slate', type: 'animated' },
    { id: 28, name: 'Burgundy', animation: 'bg-burgundy', type: 'animated' },
    { id: 29, name: 'Deep Emerald', animation: 'bg-deep-emerald', type: 'animated' },
    { id: 30, name: 'Dark Sapphire', animation: 'bg-dark-sapphire', type: 'animated' },
    { id: 31, name: 'Deep Charcoal', animation: 'bg-deep-charcoal', type: 'animated' },
    { id: 32, name: 'Dark Plum', animation: 'bg-dark-plum', type: 'animated' }
];

const videoBackgrounds: VideoBackground[] = [
    { id: 13, name: 'Snow Forest', url: '/src/assets/video/snow_forest.mp4', type: 'video' },
    { id: 14, name: 'Jellyfish', url: '/src/assets/video/jellyfish.mp4', type: 'video' },
    { id: 15, name: 'Tide', url: '/src/assets/video/tide.mp4', type: 'video' },
    { id: 16, name: 'House of Dreams', url: '/src/assets/video/house_of_dreams.mp4', type: 'video' },
    { id: 17, name: 'Coffee Shop', url: '/src/assets/video/coffee_shop.mp4', type: 'video' },
    { id: 18, name: 'City Rain', url: '/src/assets/video/city_rain.mp4', type: 'video' }
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
/* Base animations */
@keyframes gradient {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
}

@keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-10px); }
}

@keyframes pulse {
    0%, 100% { opacity: 0.5; }
    50% { opacity: 0.8; }
}

/* Background styles */
.bg-cozy-sunset {
    background: linear-gradient(45deg, #ff9a9e, #fad0c4);
    background-size: 400% 400%;
    animation: gradient 15s ease infinite;
}

.bg-ocean-waves {
    background: linear-gradient(45deg, #a8edea, #fed6e3);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-lavender-mist {
    background: linear-gradient(45deg, #e0c3fc, #8ec5fc);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-forest-dawn {
    background: linear-gradient(45deg, #d4fc79, #96e6a1);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-deep-navy {
    background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%);
    overflow: hidden;
}

.bg-deep-navy::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(255, 255, 255, 0.15) 0%, transparent 70%);
    animation: pulse 8s ease-in-out infinite;
}

.bg-ocean-deep {
    background: linear-gradient(180deg, #0f172a 0%, #1e293b 100%);
    overflow: hidden;
}

.bg-ocean-deep::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(0, 183, 255, 0.15) 0%, transparent 70%);
    animation: pulse 12s ease-in-out infinite;
}

.bg-autumn-leaves {
    background: linear-gradient(45deg, #f6d365, #fda085);
    background-size: 400% 400%;
    animation: gradient 25s ease infinite;
}

.bg-purple-dream {
    background: linear-gradient(45deg, #a18cd1, #fbc2eb);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-mint-fresh {
    background: linear-gradient(45deg, #84fab0, #8fd3f4);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-sunset-pink {
    background: linear-gradient(45deg, #ff6b6b, #ffd93d);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-midnight-blue {
    background: linear-gradient(180deg, #0f172a 0%, #1e293b 100%);
    overflow: hidden;
}

.bg-midnight-blue::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(59, 130, 246, 0.15) 0%, transparent 70%);
    animation: pulse 10s ease-in-out infinite;
}

.bg-slate-gray {
    background: linear-gradient(45deg, #f3f4f6, #e5e7eb);
    overflow: hidden;
}

.bg-slate-gray::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: linear-gradient(90deg, 
        transparent 0%,
        rgba(255, 255, 255, 0.2) 50%,
        transparent 100%
    );
    animation: float 10s ease-in-out infinite;
}

.bg-soft-coral {
    background: linear-gradient(45deg, #ffd1d1, #ff9ecd);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-sage-green {
    background: linear-gradient(45deg, #b2d732, #8cc63f);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-dusty-rose {
    background: linear-gradient(45deg, #e6a4b4, #f8d5e5);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-ocean-breeze {
    background: linear-gradient(45deg, #a8e6cf, #dcedc1);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-lavender-mist-2 {
    background: linear-gradient(45deg, #e6e6fa, #b19cd9);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-warm-sand {
    background: linear-gradient(45deg, #f4d03f, #f5ab35);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-cool-mint {
    background: linear-gradient(45deg, #98ddca, #d5ecc2);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-dusty-blue {
    background: linear-gradient(45deg, #b8c6db, #f5f7fa);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-soft-peach {
    background: linear-gradient(45deg, #ffdab9, #ffb6c1);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-forest-mist {
    background: linear-gradient(45deg, #2ecc71, #27ae60);
    background-size: 400% 400%;
    animation: gradient 20s ease infinite;
}

.bg-deep-purple {
    background: linear-gradient(180deg, #2d1b3d 0%, #1a0f2e 100%);
    overflow: hidden;
}

.bg-deep-purple::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(147, 51, 234, 0.15) 0%, transparent 70%);
    animation: pulse 10s ease-in-out infinite;
}

.bg-dark-forest {
    background: linear-gradient(180deg, #1b4332 0%, #2d6a4f 100%);
    overflow: hidden;
}

.bg-dark-forest::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(34, 197, 94, 0.15) 0%, transparent 70%);
    animation: pulse 12s ease-in-out infinite;
}

.bg-midnight-indigo {
    background: linear-gradient(180deg, #1e1b4b 0%, #312e81 100%);
    overflow: hidden;
}

.bg-midnight-indigo::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(99, 102, 241, 0.15) 0%, transparent 70%);
    animation: pulse 8s ease-in-out infinite;
}

.bg-deep-ocean-2 {
    background: linear-gradient(180deg, #0c4a6e 0%, #0369a1 100%);
    overflow: hidden;
}

.bg-deep-ocean-2::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(14, 165, 233, 0.15) 0%, transparent 70%);
    animation: pulse 15s ease-in-out infinite;
}

.bg-dark-slate {
    background: linear-gradient(180deg, #1e293b 0%, #334155 100%);
    overflow: hidden;
}

.bg-dark-slate::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(148, 163, 184, 0.15) 0%, transparent 70%);
    animation: pulse 10s ease-in-out infinite;
}

.bg-burgundy {
    background: linear-gradient(180deg, #7f1d1d 0%, #991b1b 100%);
    overflow: hidden;
}

.bg-burgundy::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(239, 68, 68, 0.15) 0%, transparent 70%);
    animation: pulse 12s ease-in-out infinite;
}

.bg-deep-emerald {
    background: linear-gradient(180deg, #064e3b 0%, #065f46 100%);
    overflow: hidden;
}

.bg-deep-emerald::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(16, 185, 129, 0.15) 0%, transparent 70%);
    animation: pulse 10s ease-in-out infinite;
}

.bg-dark-sapphire {
    background: linear-gradient(180deg, #172554 0%, #1e3a8a 100%);
    overflow: hidden;
}

.bg-dark-sapphire::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(59, 130, 246, 0.15) 0%, transparent 70%);
    animation: pulse 8s ease-in-out infinite;
}

.bg-deep-charcoal {
    background: linear-gradient(180deg, #18181b 0%, #27272a 100%);
    overflow: hidden;
}

.bg-deep-charcoal::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(161, 161, 170, 0.15) 0%, transparent 70%);
    animation: pulse 12s ease-in-out infinite;
}

.bg-dark-plum {
    background: linear-gradient(180deg, #581c87 0%, #6b21a8 100%);
    overflow: hidden;
}

.bg-dark-plum::before {
    content: '';
    position: absolute;
    width: 100%;
    height: 100%;
    background: radial-gradient(circle at 50% 50%, rgba(168, 85, 247, 0.15) 0%, transparent 70%);
    animation: pulse 10s ease-in-out infinite;
}

/* Custom scrollbar styles */
.custom-scrollbar {
    scrollbar-width: thin;
    scrollbar-color: rgba(255, 255, 255, 0.2) transparent;
    padding-right: 8px;
}

.custom-scrollbar::-webkit-scrollbar {
    width: 6px;
}

.custom-scrollbar::-webkit-scrollbar-track {
    background: transparent;
}

.custom-scrollbar::-webkit-scrollbar-thumb {
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 3px;
}

.custom-scrollbar::-webkit-scrollbar-thumb:hover {
    background-color: rgba(255, 255, 255, 0.3);
}

/* Keep horizontal scrollbar hidden */
.scrollbar-hide {
    -ms-overflow-style: none;
    scrollbar-width: none;
}

.scrollbar-hide::-webkit-scrollbar {
    display: none;
}

/* Add padding to ensure the last tab is fully visible */
.scrollbar-hide::after {
    content: '';
    display: block;
    width: 1px;
    height: 1px;
    padding-right: 1px;
}
</style> 