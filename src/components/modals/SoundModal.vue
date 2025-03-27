<template>
    <div 
        id="sound-modal"
        class="fixed inset-y-0 right-0 w-[400px] bg-[#1a1a2e]/95 backdrop-blur-xl transform transition-transform duration-300 ease-in-out z-50 border-l border-white/10"
        :class="modelValue ? 'translate-x-0' : 'translate-x-full'"
    >
        <div class="h-full flex flex-col p-6">
            <!-- Header -->
            <div class="flex justify-between items-center mb-6">
                <div>
                    <h2 class="text-xl font-semibold text-white">Ambient Sound Settings</h2>
                    <p class="text-sm text-white/60">Choose your ambient sounds</p>
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
                <div class="space-y-4">
                    <!-- Sound Options -->
                    <div v-for="i in 5" :key="i" class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-colors cursor-pointer">
                        <div class="flex items-center gap-4">
                            <div class="w-12 h-12 bg-indigo-500/20 rounded-lg flex items-center justify-center">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                                </svg>
                            </div>
                            <div class="flex-1">
                                <h3 class="text-white font-medium">Ambient Sound {{ i }}</h3>
                                <p class="text-white/60 text-sm">Relaxing sounds to focus</p>
                            </div>
                            <button class="text-white/60 hover:text-white">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14.752 11.168l-3.197-2.132A1 1 0 0010 9.87v4.263a1 1 0 001.555.832l3.197-2.132a1 1 0 000-1.664z" />
                                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                                </svg>
                            </button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup lang="ts">
defineProps<{
    modelValue: boolean
}>();

defineEmits<{
    (e: 'update:modelValue', value: boolean): void
}>();
</script> 