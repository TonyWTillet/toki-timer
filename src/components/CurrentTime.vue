<template>
    <div class="text-center">
        <div class="text-4xl font-bold text-white mb-2">{{ currentTime }}</div>
        <div class="text-lg text-white/60">{{ currentDate }}</div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';

const currentTime = ref<string>('');
const currentDate = ref<string>('');
let animationFrameId: number;

const updateTime = () => {
    const now = new Date();
    
    // Format time (HH:MM) using user's locale and timezone
    currentTime.value = now.toLocaleTimeString(undefined, {
        hour: '2-digit',
        minute: '2-digit',
        second: '2-digit',
        hour12: false
    });
    
    // Format date using user's locale and timezone
    currentDate.value = now.toLocaleDateString(undefined, {
        day: '2-digit',
        month: '2-digit',
        year: 'numeric'
    });

    // Request next frame
    animationFrameId = requestAnimationFrame(updateTime);
};

// Start animation on mount
onMounted(() => {
    updateTime();
});

// Cleanup on unmount
onUnmounted(() => {
    if (animationFrameId) {
        cancelAnimationFrame(animationFrameId);
    }
});
</script> 