<template>
  <div>
    <!-- Stories Ring (Shows on home page) -->
    <div v-if="stories.length > 0" class="max-w-7xl mx-auto px-3 sm:px-4 lg:px-6 mb-4 sm:mb-6">
      <div class="flex gap-3 overflow-x-auto pb-2 scrollbar-hide">
        <button
          v-for="(story, idx) in stories"
          :key="story.id"
          @click="openStories(idx)"
          class="flex-shrink-0 flex flex-col items-center gap-2 group"
        >
          <!-- Story Ring -->
          <div class="relative">
            <div class="w-16 h-16 sm:w-20 sm:h-20 rounded-full bg-gradient-to-tr from-purple-500 via-pink-500 to-red-500 p-0.5 group-hover:scale-110 transition-transform duration-300">
              <div class="w-full h-full rounded-full p-0.5"
                :class="darkMode ? 'bg-slate-900' : 'bg-white'">
                <div 
                  class="w-full h-full rounded-full bg-cover bg-center bg-gray-300"
                  :style="{ backgroundImage: `url(${getStoryThumbnail(story)})` }"
                ></div>
              </div>
            </div>
            <!-- New badge -->
            <div class="absolute -bottom-1 -right-1 w-5 h-5 bg-gradient-to-r from-purple-500 to-pink-500 rounded-full flex items-center justify-center text-white text-xs font-bold shadow-lg">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-3 h-3" fill="currentColor" viewBox="0 0 24 24">
                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
              </svg>
            </div>
          </div>
          <!-- Author name -->
          <span class="text-xs font-medium max-w-[70px] truncate"
            :class="darkMode ? 'text-gray-300' : 'text-gray-700'">
            {{ story.author?.username || 'MeteorDub' }}
          </span>
        </button>
      </div>
    </div>

    <!-- Stories Viewer Modal -->
    <Transition name="fade">
      <div
        v-if="isOpen"
        class="fixed inset-0 z-[9999] bg-black"
        @click.self="closeStories"
      >
        <!-- Close Button -->
        <button
          @click="closeStories"
          class="absolute top-4 right-4 z-50 w-10 h-10 rounded-full bg-black/50 backdrop-blur-sm flex items-center justify-center text-white hover:bg-black/70 transition-colors"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
          </svg>
        </button>

        <!-- Story Content Container -->
        <div class="relative h-full max-w-md mx-auto flex flex-col">
          <!-- Progress Bars -->
          <div class="absolute top-0 left-0 right-0 z-40 flex gap-1 p-2">
            <div
              v-for="(story, idx) in stories"
              :key="story.id"
              class="flex-1 h-0.5 bg-white/30 rounded-full overflow-hidden"
            >
              <div
                class="h-full bg-white transition-all duration-100 ease-linear"
                :style="{ width: getProgressWidth(idx) + '%' }"
              ></div>
            </div>
          </div>

          <!-- Story Header -->
          <div class="absolute top-8 left-0 right-0 z-40 flex items-center gap-3 px-4 py-3">
            <div class="w-10 h-10 rounded-full bg-gradient-to-tr from-purple-500 to-pink-500 p-0.5">
              <div 
                class="w-full h-full rounded-full bg-cover bg-center bg-gray-300"
                :style="{ backgroundImage: `url(${getStoryThumbnail(currentStory)})` }"
              ></div>
            </div>
            <div class="flex-1">
              <p class="text-white font-semibold text-sm">
                {{ currentStory?.author?.username || 'MeteorDub' }}
              </p>
              <p class="text-white/70 text-xs">
                {{ getTimeAgo(currentStory?.created_at) }}
              </p>
            </div>
            <!-- Pause/Play Button -->
            <button
              @click="togglePause"
              class="w-8 h-8 rounded-full bg-black/30 backdrop-blur-sm flex items-center justify-center text-white hover:bg-black/50 transition-colors"
            >
              <svg v-if="isPaused" xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                <path d="M8 5v14l11-7z"/>
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                <path d="M6 4h4v16H6V4zm8 0h4v16h-4V4z"/>
              </svg>
            </button>
            <!-- Mute Button (for videos) -->
            <button
              v-if="currentStory?.media_type === 'video'"
              @click="toggleMute"
              class="w-8 h-8 rounded-full bg-black/30 backdrop-blur-sm flex items-center justify-center text-white hover:bg-black/50 transition-colors"
            >
              <svg v-if="isMuted" xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2" />
              </svg>
              <svg v-else xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.536 8.464a5 5 0 010 7.072m2.828-9.9a9 9 0 010 12.728M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z" />
              </svg>
            </button>
          </div>

          <!-- Story Content -->
          <div
            class="flex-1 relative flex items-center justify-center"
            @touchstart="handleTouchStart"
            @touchend="handleTouchEnd"
          >
            <!-- Loading Spinner -->
            <div v-if="isLoadingMedia" class="absolute inset-0 flex items-center justify-center">
              <div class="w-12 h-12 border-4 border-white/30 border-t-white rounded-full animate-spin"></div>
            </div>

            <!-- Image Story -->
            <img
              v-if="currentStory?.media_type === 'image'"
              :src="currentStory.media"
              :alt="currentStory.title"
              class="max-w-full max-h-full object-contain"
              @load="handleMediaLoad"
            />

            <!-- Video Story -->
            <video
              v-else-if="currentStory?.media_type === 'video'"
              ref="videoRef"
              :src="currentStory.media"
              class="max-w-full max-h-full object-contain"
              :muted="isMuted"
              autoplay
              playsinline
              @loadeddata="handleMediaLoad"
              @ended="nextStory"
            ></video>

            <!-- Text Story -->
            <div
              v-else-if="currentStory?.media_type === 'text'"
              class="w-full h-full flex items-center justify-center p-8"
              :style="{ background: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)' }"
            >
              <div class="text-center">
                <h2 v-if="currentStory.title" class="text-3xl font-bold text-white mb-4">
                  {{ currentStory.title }}
                </h2>
                <p class="text-xl text-white/90 leading-relaxed">
                  {{ currentStory.text }}
                </p>
              </div>
            </div>

            <!-- Navigation Areas (invisible) -->
            <div class="absolute inset-0 flex">
              <button
                @click.stop="prevStory"
                class="flex-1 cursor-pointer"
                aria-label="Previous story"
              ></button>
              <button
                @click.stop="nextStory"
                class="flex-1 cursor-pointer"
                aria-label="Next story"
              ></button>
            </div>
          </div>

          <!-- Story Footer (if title exists) -->
          <div v-if="currentStory?.title && currentStory?.media_type !== 'text'" class="absolute bottom-0 left-0 right-0 z-40 p-4 bg-gradient-to-t from-black/80 to-transparent">
            <h3 class="text-white font-semibold text-lg">{{ currentStory.title }}</h3>
          </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted, watch } from 'vue';

const API_BASE = 'https://api.meteordub.uz/api';

// Props
const props = defineProps({
  darkMode: {
    type: Boolean,
    default: false
  }
});

// State
const stories = ref([]);
const currentStoryIndex = ref(0);
const progress = ref(0);
const isPaused = ref(false);
const isOpen = ref(false);
const isMuted = ref(true);
const isLoadingMedia = ref(false);
const touchStart = ref(null);
const videoRef = ref(null);

let progressInterval = null;
const STORY_DURATION = 5000;
const PROGRESS_UPDATE_INTERVAL = 50;

// Computed
const currentStory = computed(() => stories.value[currentStoryIndex.value]);

// Fetch stories from API
const fetchStories = async () => {
  try {
    const response = await fetch(`${API_BASE}/stories/`);
    const data = await response.json();
    // API returns 'results' not 'data'
    if (data.results && data.results.length > 0) {
      stories.value = data.results;
      console.log('✅ Stories loaded:', stories.value.length);
    } else {
      console.log('ℹ️ No stories available');
    }
  } catch (error) {
    console.error('❌ Error fetching stories:', error);
  }
};

// Get story thumbnail
const getStoryThumbnail = (story) => {
  if (!story) return '';
  if (story.media_type === 'image' && story.media) {
    return story.media;
  }
  if (story.media_type === 'video' && story.media) {
    return story.media;
  }
  return '';
};

// Get time ago
const getTimeAgo = (dateString) => {
  if (!dateString) return '';
  const now = new Date();
  const date = new Date(dateString);
  const seconds = Math.floor((now - date) / 1000);
  
  if (seconds < 60) return 'just now';
  if (seconds < 3600) return `${Math.floor(seconds / 60)}m ago`;
  if (seconds < 86400) return `${Math.floor(seconds / 3600)}h ago`;
  return `${Math.floor(seconds / 86400)}d ago`;
};

// Get progress width for each story
const getProgressWidth = (idx) => {
  if (idx < currentStoryIndex.value) return 100;
  if (idx > currentStoryIndex.value) return 0;
  return progress.value;
};

// Open stories viewer
const openStories = (idx) => {
  currentStoryIndex.value = idx;
  isOpen.value = true;
  progress.value = 0;
  isPaused.value = false;
  isLoadingMedia.value = true;
  document.body.style.overflow = 'hidden';
  startProgress();
  recordView(stories.value[idx].id);
};

// Close stories viewer
const closeStories = () => {
  isOpen.value = false;
  document.body.style.overflow = '';
  stopProgress();
  if (videoRef.value) {
    videoRef.value.pause();
  }
};

// Start progress
const startProgress = () => {
  stopProgress();
  progressInterval = setInterval(() => {
    if (!isPaused.value && !isLoadingMedia.value) {
      progress.value += (PROGRESS_UPDATE_INTERVAL / STORY_DURATION) * 100;
      if (progress.value >= 100) {
        nextStory();
      }
    }
  }, PROGRESS_UPDATE_INTERVAL);
};

// Stop progress
const stopProgress = () => {
  if (progressInterval) {
    clearInterval(progressInterval);
    progressInterval = null;
  }
};

// Next story
const nextStory = () => {
  if (currentStoryIndex.value < stories.value.length - 1) {
    currentStoryIndex.value++;
    progress.value = 0;
    isLoadingMedia.value = true;
    recordView(stories.value[currentStoryIndex.value].id);
  } else {
    closeStories();
  }
};

// Previous story
const prevStory = () => {
  if (progress.value > 10) {
    progress.value = 0;
    isLoadingMedia.value = true;
  } else if (currentStoryIndex.value > 0) {
    currentStoryIndex.value--;
    progress.value = 0;
    isLoadingMedia.value = true;
    recordView(stories.value[currentStoryIndex.value].id);
  }
};

// Toggle pause
const togglePause = () => {
  isPaused.value = !isPaused.value;
  if (videoRef.value) {
    if (isPaused.value) {
      videoRef.value.pause();
    } else {
      videoRef.value.play();
    }
  }
};

// Toggle mute
const toggleMute = () => {
  isMuted.value = !isMuted.value;
  if (videoRef.value) {
    videoRef.value.muted = isMuted.value;
  }
};

// Handle media load
const handleMediaLoad = () => {
  isLoadingMedia.value = false;
};

// Touch handlers for swipe
const handleTouchStart = (e) => {
  touchStart.value = e.touches[0].clientX;
};

const handleTouchEnd = (e) => {
  if (!touchStart.value) return;
  
  const touchEnd = e.changedTouches[0].clientX;
  const diff = touchStart.value - touchEnd;
  
  if (Math.abs(diff) > 50) {
    if (diff > 0) {
      nextStory();
    } else {
      prevStory();
    }
  }
  
  touchStart.value = null;
};

// Record story view
const recordView = async (storyId) => {
  try {
    let sessionId = localStorage.getItem('sessionId');
    if (!sessionId) {
      sessionId = 'session_' + Math.random().toString(36).substr(2, 9) + Date.now();
      localStorage.setItem('sessionId', sessionId);
    }
    
    await fetch(`${API_BASE}/stories/${storyId}/view/`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-Session-Id': sessionId,
      },
    });
  } catch (error) {
    console.error('❌ Error recording view:', error);
  }
};

// Watch for story changes
watch(currentStoryIndex, () => {
  if (videoRef.value) {
    videoRef.value.load();
    videoRef.value.play();
  }
});

// Keyboard navigation
const handleKeyPress = (e) => {
  if (!isOpen.value) return;
  
  if (e.key === 'ArrowRight') nextStory();
  if (e.key === 'ArrowLeft') prevStory();
  if (e.key === 'Escape') closeStories();
  if (e.key === ' ') {
    e.preventDefault();
    togglePause();
  }
};

// Lifecycle
onMounted(() => {
  fetchStories();
  window.addEventListener('keydown', handleKeyPress);
});

onUnmounted(() => {
  stopProgress();
  document.body.style.overflow = '';
  window.removeEventListener('keydown', handleKeyPress);
});
</script>

<style scoped>
/* Hide scrollbar */
.scrollbar-hide::-webkit-scrollbar {
  display: none;
}

.scrollbar-hide {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

/* Fade transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

/* Prevent text selection */
button,
div {
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}
</style>