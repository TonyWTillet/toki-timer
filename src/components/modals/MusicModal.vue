<template>
    <AudioModalBase
        :modelValue="modelValue"
        id="music-modal"
        title="Music Settings"
        subtitle="Choose your background music"
        @volume-change="handleVolumeChange"
        @update:modelValue="$emit('update:modelValue', $event)"
    >
        <template #content>
            <!-- Music Options -->
            <div 
                v-for="track in musicYoutube" 
                :key="track.id" 
                class="bg-white/5 rounded-lg p-4 hover:bg-white/10 transition-colors cursor-pointer"
                :class="{ 'bg-indigo-500/20': currentTrack?.id === track.id }"
                @click="handleTrackClick(track)"
            >
                <div class="flex items-center gap-4">
                    <div class="w-12 h-12 bg-indigo-500/20 rounded-lg flex items-center justify-center">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19V6l12-3v13M9 19c0 1.105-1.343 2-3 2s-3-.895-3-2 1.343-2 3-2 3 .895 3 2z" />
                        </svg>
                    </div>
                    <div class="flex-1">
                        <h3 class="text-white font-medium">{{ track.name }}</h3>
                        <p class="text-white/60 text-sm">{{ currentTrack?.id === track.id ? (isPlaying ? 'Playing' : 'Paused') : 'Click to play' }}</p>
                    </div>
                    <button 
                        class="text-white/60 hover:text-white"
                        @click.stop="handleTrackClick(track)"
                    >
                        <svg v-if="currentTrack?.id === track.id && isPlaying" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 9v6m4-6v6m7-3a9 9 0 11-18 0 9 9 0 0118 0z" />
                        </svg>
                        <svg v-else-if="currentTrack?.id === track.id && !isPlaying" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
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

    <!-- Hidden YouTube Player -->
    <iframe
        ref="youtubePlayer"
        class="hidden"
        width="0"
        height="0"
        :src="`https://www.youtube.com/embed/${currentVideoId}?autoplay=1&enablejsapi=1&controls=0&loop=1&playlist=${currentVideoId}`"
        allow="autoplay"
    ></iframe>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
import AudioModalBase from './AudioModalBase.vue';

interface Track {
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

const musicYoutube = ref<Track[]>([
    { id: 1, name: 'Lofi Study radio', url: 'https://www.youtube.com/watch?v=jfKfPfyJRdk' },
    { id: 2, name: 'Lofi Jazz radio', url: 'https://www.youtube.com/watch?v=HuFYqnbVbzY' },
    { id: 3, name: 'Lofi Dark ambient radio', url: 'https://www.youtube.com/watch?v=S_MOd40zlYU' },
]);

const youtubePlayer = ref<HTMLIFrameElement | null>(null);
const currentTrack = ref<Track | null>(null);
const currentVideoId = ref<string>('');
const isPlaying = ref(false);
let player: any = null;

// Extract video ID from YouTube URL
const getVideoId = (url: string): string => {
    const regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|&v=)([^#&?]*).*/;
    const match = url.match(regExp);
    return match && match[2].length === 11 ? match[2] : '';
};

// Handle track click
const handleTrackClick = (track: Track) => {
    if (!player) return;

    if (currentTrack.value?.id === track.id) {
        // Si c'est la même piste, on bascule play/pause
        if (isPlaying.value) {
            player.pauseVideo();
            isPlaying.value = false;
        } else {
            player.playVideo();
            isPlaying.value = true;
        }
    } else {
        // Si c'est une nouvelle piste, on la lance
        currentTrack.value = track;
        currentVideoId.value = getVideoId(track.url);
        isPlaying.value = true;
        
        // Forcer le rechargement de la vidéo
        player.loadVideoById({
            videoId: currentVideoId.value,
            playerVars: {
                autoplay: 1,
                controls: 0,
                loop: 1,
                playlist: currentVideoId.value
            }
        });
    }
};

// Handle volume change
const handleVolumeChange = (newVolume: number) => {
    if (player) {
        player.setVolume(newVolume);
    }
};

// Initialize YouTube Player API
const initYouTubePlayer = () => {
    if (typeof window.YT === 'undefined' || !youtubePlayer.value) return;

    player = new window.YT.Player(youtubePlayer.value, {
        events: {
            onReady: () => {
                player.setVolume(50);
            },
            onStateChange: (event: any) => {
                if (event.data === window.YT.PlayerState.PLAYING) {
                    isPlaying.value = true;
                } else if (event.data === window.YT.PlayerState.PAUSED) {
                    isPlaying.value = false;
                } else if (event.data === window.YT.PlayerState.ENDED) {
                    isPlaying.value = false;
                    if (currentVideoId.value) {
                        player.loadVideoById({
                            videoId: currentVideoId.value,
                            playerVars: {
                                autoplay: 1,
                                controls: 0,
                                loop: 1,
                                playlist: currentVideoId.value
                            }
                        });
                    }
                }
            }
        }
    });
};

// Load YouTube API
const loadYouTubeAPI = () => {
    const tag = document.createElement('script');
    tag.src = 'https://www.youtube.com/iframe_api';
    const firstScriptTag = document.getElementsByTagName('script')[0];
    firstScriptTag.parentNode?.insertBefore(tag, firstScriptTag);
};

// Initialize YouTube API on component mount
onMounted(() => {
    loadYouTubeAPI();
    window.onYouTubeIframeAPIReady = initYouTubePlayer;
});

// Cleanup on component unmount
onUnmounted(() => {
    if (player) {
        player.destroy();
    }
});
</script>

<style>
/* Add YouTube API types */
declare global {
    interface Window {
        YT: any;
        onYouTubeIframeAPIReady: () => void;
    }
}
</style> 