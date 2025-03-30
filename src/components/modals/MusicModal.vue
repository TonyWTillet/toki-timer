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
                        <svg xmlns="http://www.w3.org/2000/svg" x="0px" y="0px" width="50" height="50" viewBox="0 0 32 32"
                            style="fill:#ed8cff;">
                            <path d="M 23.068359 5.0058594 C 22.890359 5.0058594 21.625813 5.2552656 11.257812 7.1972656 C 11.176812 7.1972656 10.789172 7.2985625 10.576172 7.4765625 C 10.367172 7.6445625 10.255844 7.8688125 10.214844 8.1328125 C 10.041844 8.8238125 10.593078 21.281234 10.080078 21.865234 C 9.8560781 22.114234 9.46125 22.186187 9.15625 22.242188 C 7.33225 22.562188 6.1179062 22.729734 5.2539062 23.552734 C 3.7649063 25.005734 4.5364219 27.541125 6.7324219 27.953125 C 7.5554219 28.105125 9.1010781 27.898781 9.9550781 27.425781 C 10.707078 27.038781 11.277234 26.342969 11.490234 25.417969 C 11.673234 24.624969 11.617188 25.173141 11.617188 13.244141 C 11.617188 12.659141 11.780828 12.506531 12.298828 12.394531 C 12.298828 12.394531 21.319328 10.707719 21.736328 10.636719 C 22.320328 10.534719 22.599609 10.692719 22.599609 11.261719 C 22.599609 19.348719 22.701953 19.190641 22.376953 19.556641 C 22.152953 19.805641 21.887031 19.875641 21.582031 19.931641 C 19.757031 20.251641 18.655016 20.307859 17.791016 21.130859 C 16.698016 22.197859 16.712734 23.890109 17.927734 24.912109 C 18.832734 25.602109 19.84875 25.771969 21.09375 25.542969 C 22.51175 25.277969 23.569531 24.494563 23.894531 23.101562 C 24.041531 22.466562 23.994281 23.406391 23.988281 5.9003906 C 23.988281 5.3103906 23.652359 4.9648594 23.068359 5.0058594 z"></path>
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
    { id: 4, name: 'Lofi Asian radio', url: 'https://www.youtube.com/watch?v=Na0w3Mz46GA' },
    { id: 5, name: 'Lofi Piano radio', url: 'https://www.youtube.com/watch?v=TtkFsfOP9QI' },
    { id: 6, name: 'Lofi Synthwave radio', url: 'https://www.youtube.com/watch?v=4xDzrJKXOOY' },
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