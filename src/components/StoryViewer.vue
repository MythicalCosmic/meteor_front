<template>
  <div>
    <!-- Official Stories Banner -->
    <div v-if="stories.length > 0" class="max-w-7xl mx-auto px-3 sm:px-4 lg:px-6 mb-4 sm:mb-6">
      <!-- Main Featured Story Banner -->
      <div 
        @click="openStories(0)"
        class="relative overflow-hidden rounded-2xl sm:rounded-3xl cursor-pointer group transition-all duration-300 hover:scale-[1.02] active:scale-[0.98]"
        :class="darkMode 
          ? 'bg-gradient-to-r from-purple-900/50 via-pink-900/50 to-red-900/50 border border-purple-700/50' 
          : 'bg-gradient-to-r from-purple-50 via-pink-50 to-red-50 border border-purple-200'"
      >
        <!-- Animated Background Pattern -->
        <div class="absolute inset-0 opacity-10">
          <div class="absolute inset-0 bg-[radial-gradient(circle_at_50%_50%,rgba(255,255,255,0.1),transparent_50%)] animate-pulse"></div>
        </div>

        <div class="relative flex items-center gap-3 sm:gap-4 p-3 sm:p-4">
          <!-- Animated Icon -->
          <div class="flex-shrink-0">
            <div class="relative">
              <div class="w-12 h-12 sm:w-16 sm:h-16 rounded-2xl bg-gradient-to-br from-purple-500 via-pink-500 to-red-500 flex items-center justify-center shadow-lg group-hover:shadow-xl transition-shadow">
                <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6 sm:w-8 sm:h-8 text-white animate-pulse" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
                </svg>
              </div>
              <!-- Ping animation -->
              <div class="absolute inset-0 rounded-2xl bg-gradient-to-br from-purple-500 to-pink-500 animate-ping opacity-20"></div>
            </div>
          </div>

          <!-- Content -->
          <div class="flex-1 min-w-0">
            <div class="flex items-center gap-2 mb-1">
              <span class="px-2 sm:px-3 py-0.5 sm:py-1 rounded-full text-xs font-bold bg-gradient-to-r from-purple-500 to-pink-500 text-white shadow-lg">
                📢 NEW
              </span>
              <span class="text-xs opacity-60" :class="darkMode ? 'text-gray-400' : 'text-gray-600'">
                {{ stories.length }} {{ stories.length === 1 ? 'announcement' : 'announcements' }}
              </span>
            </div>
            <h3 class="font-bold text-sm sm:text-base md:text-lg mb-0.5 sm:mb-1 truncate"
              :class="darkMode ? 'text-white' : 'text-gray-900'">
              {{ stories[0].title || 'Official Announcement' }}
            </h3>
            <p class="text-xs sm:text-sm opacity-70 line-clamp-1"
              :class="darkMode ? 'text-gray-300' : 'text-gray-700'">
              Tap to view {{ stories.length > 1 ? 'all announcements' : 'announcement' }}
            </p>
          </div>

          <!-- Arrow with animation -->
          <div class="flex-shrink-0">
            <div class="w-8 h-8 sm:w-10 sm:h-10 rounded-full flex items-center justify-center transition-transform group-hover:translate-x-1"
              :class="darkMode ? 'bg-white/10' : 'bg-gray-200'">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4 sm:w-5 sm:h-5" :class="darkMode ? 'text-white' : 'text-gray-900'" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>
            </div>
          </div>
        </div>

        <!-- Bottom indicator if multiple stories -->
        <div v-if="stories.length > 1" class="absolute bottom-0 left-0 right-0 h-1 bg-gradient-to-r from-purple-500 via-pink-500 to-red-500 opacity-50"></div>
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
            <div class="w-10 h-10 rounded-full bg-gradient-to-tr from-purple-500 to-pink-500 flex items-center justify-center shadow-lg">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 text-white" fill="currentColor" viewBox="0 0 24 24">
                <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
              </svg>
            </div>
            <div class="flex-1">
              <p class="text-white font-semibold text-sm flex items-center gap-2">
                MeteorDub Official
                <span class="px-2 py-0.5 rounded-full bg-blue-500 text-white text-xs">✓</span>
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
              @timeupdate="handleVideoProgress"
            ></video>

            <!-- Text Story -->
            <div
              v-else-if="currentStory?.media_type === 'text'"
              class="w-full h-full flex items-center justify-center p-8"
              :style="{ background: 'linear-gradient(135deg, #667eea 0%, #764ba2 100%)' }"
            >
              <div class="text-center max-w-lg">
                <div class="w-16 h-16 mx-auto mb-6 rounded-full bg-white/20 backdrop-blur-sm flex items-center justify-center">
                  <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8 text-white" fill="currentColor" viewBox="0 0 24 24">
                    <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
                  </svg>
                </div>
                <h2 v-if="currentStory.title" class="text-2xl sm:text-3xl font-bold text-white mb-4">
                  {{ currentStory.title }}
                </h2>
                <p class="text-lg sm:text-xl text-white/90 leading-relaxed">
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
          <div v-if="currentStory?.title && currentStory?.media_type !== 'text'" class="absolute bottom-0 left-0 right-0 z-40 p-6 bg-gradient-to-t from-black/90 via-black/60 to-transparent">
            <div class="flex items-start gap-3">
              <div class="flex-shrink-0 w-10 h-10 rounded-full bg-gradient-to-br from-purple-500 to-pink-500 flex items-center justify-center">
                <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 text-white" fill="currentColor" viewBox="0 0 24 24">
                  <path d="M11 5v11.17l-4.88-4.88c-.39-.39-1.03-.39-1.42 0-.39.39-.39 1.02 0 1.41l6.59 6.59c.39.39 1.02.39 1.41 0l6.59-6.59c.39-.39.39-1.02 0-1.41-.39-.39-1.02-.39-1.41 0L13 16.17V5c0-.55-.45-1-1-1s-1 .45-1 1z"/>
                </svg>
              </div>
              <div class="flex-1">
                <h3 class="text-white font-bold text-lg mb-1">{{ currentStory.title }}</h3>
                <p v-if="currentStory.text" class="text-white/80 text-sm line-clamp-2">{{ currentStory.text }}</p>
              </div>
            </div>
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
  
  // For videos, don't use timer - let video @ended event handle it
  if (currentStory.value?.media_type === 'video') {
    return;
  }
  
  // For images and text, use 5 second timer
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
  // Restart progress for new story
  if (currentStory.value?.media_type !== 'video') {
    startProgress();
  }
};

// Handle video progress for progress bar
const handleVideoProgress = () => {
  if (videoRef.value && currentStory.value?.media_type === 'video') {
    const percent = (videoRef.value.currentTime / videoRef.value.duration) * 100;
    progress.value = percent;
  }
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
  // Restart progress for new story
  progress.value = 0;
  isLoadingMedia.value = true;
  startProgress();
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

/* Line clamp */
.line-clamp-1 {
  display: -webkit-box;
  -webkit-line-clamp: 1;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Prevent text selection */
button,
div {
  -webkit-tap-highlight-color: transparent;
  user-select: none;
}
</style>