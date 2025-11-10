<template>
  <div
    @click="goToAnime"
    class="group relative rounded-xl overflow-hidden cursor-pointer transition-all duration-300 hover:scale-[1.02]"
  >
    <!-- Main Card Container -->
    <div
      class="relative aspect-[2/3] overflow-hidden rounded-xl shadow-lg transition-all duration-300"
      :class="darkMode 
        ? 'bg-slate-800 shadow-slate-900/50 group-hover:shadow-purple-500/30' 
        : 'bg-gray-100 shadow-gray-300/50 group-hover:shadow-purple-400/30'"
    >
      <!-- Image -->
      <img
        :src="imageSrc"
        :alt="animeTitle"
        class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-105"
        loading="lazy"
      />

      <!-- Gradient Overlay -->
      <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent"></div>

      <!-- Top Badges -->
      <div class="absolute top-2 left-2 flex flex-col gap-1.5 z-10">
        <!-- Rating Badge -->
        <div
          v-if="anime.rating && anime.rating > 0"
          class="flex items-center gap-1 px-2 py-0.5 bg-yellow-400 rounded-md shadow-md"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3 text-yellow-900" fill="currentColor" viewBox="0 0 24 24">
            <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
          </svg>
          <span class="text-xs font-bold text-yellow-900">{{ anime.rating.toFixed(1) }}</span>
        </div>

        <!-- Views Badge -->
        <div
          v-if="anime.total_views !== undefined"
          class="flex items-center gap-1 px-2 py-0.5 bg-purple-500 rounded-md shadow-md"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3 text-white" fill="currentColor" viewBox="0 0 24 24">
            <path d="M12 4.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5zM12 17c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5zm0-8c-1.66 0-3 1.34-3 3s1.34 3 3 3 3-1.34 3-3-1.34-3-3-3z"/>
          </svg>
          <span class="text-xs font-bold text-white">{{ formatViews(anime.total_views) }}</span>
        </div>

        <!-- Status Badge -->
        <div
          v-if="anime.status"
          class="px-2 py-0.5 rounded-md text-xs font-semibold shadow-md"
          :class="anime.status === 'ONGOING' || anime.status === 'ongoing'
            ? 'bg-green-500 text-white'
            : anime.status === 'COMPLETED'
            ? 'bg-blue-500 text-white'
            : 'bg-gray-500 text-white'"
        >
          <span class="flex items-center gap-1">
            <span v-if="anime.status === 'ONGOING' || anime.status === 'ongoing'" class="w-1.5 h-1.5 bg-white rounded-full"></span>
            {{ anime.status === 'ONGOING' || anime.status === 'ongoing' ? 'Ongoing' : anime.status === 'COMPLETED' ? 'Completed' : anime.status }}
          </span>
        </div>
      </div>

      <!-- Type Badge (Top Right) -->
      <div
        v-if="anime.type"
        class="absolute top-2 right-2 px-2 py-0.5 rounded-md text-xs font-semibold bg-white/90 text-gray-800 shadow-md z-10"
      >
        {{ anime.type }}
      </div>

      <!-- Play Overlay (appears on hover) -->
      <div class="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity duration-300 z-10">
        <div class="w-16 h-16 rounded-full bg-white/90 flex items-center justify-center shadow-xl">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-purple-600 ml-1" fill="currentColor" viewBox="0 0 24 24">
            <path d="M8 5v14l11-7z"/>
          </svg>
        </div>
      </div>

      <!-- Bottom Info -->
      <div class="absolute bottom-0 left-0 right-0 p-3 z-10">
        <!-- Title -->
        <h3 class="text-white font-bold text-sm sm:text-base leading-tight mb-2 line-clamp-2">
          {{ animeTitle }}
        </h3>

        <!-- Metadata -->
        <div class="flex items-center gap-2 text-xs text-white/90 opacity-0 group-hover:opacity-100 transition-opacity duration-300">
          <span class="flex items-center gap-1 bg-white/20 backdrop-blur-sm px-2 py-0.5 rounded">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
            </svg>
            {{ anime.release_year || 'N/A' }}
          </span>
          
          <span v-if="anime.total_episodes && anime.total_episodes > 0" class="flex items-center gap-1 bg-white/20 backdrop-blur-sm px-2 py-0.5 rounded">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-3 w-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 4v16M17 4v16M3 8h4m10 0h4M3 12h18M3 16h4m10 0h4M4 20h16a1 1 0 001-1V5a1 1 0 00-1-1H4a1 1 0 00-1 1v14a1 1 0 001 1z" />
            </svg>
            {{ anime.total_episodes }} {{ anime.total_episodes === 1 ? 'ep' : 'eps' }}
          </span>
        </div>

        <!-- Progress Bar (if watching) -->
        <div v-if="watchProgress > 0" class="mt-2 opacity-0 group-hover:opacity-100 transition-opacity duration-300">
          <div class="w-full h-1 bg-white/20 rounded-full overflow-hidden">
            <div 
              class="h-full bg-purple-500 rounded-full transition-all duration-300"
              :style="{ width: `${watchProgress}%` }"
            ></div>
          </div>
          <p class="text-xs text-white/80 mt-1">{{ watchProgress }}% watched</p>
        </div>
      </div>
    </div>

    <!-- Premium Badge (if premium) -->
    <div
      v-if="anime.is_premium_only"
      class="absolute -top-2 -right-2 z-20"
    >
      <div class="w-8 h-8 bg-yellow-400 rounded-full flex items-center justify-center shadow-lg border-2 border-white">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-yellow-900" fill="currentColor" viewBox="0 0 24 24">
          <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
        </svg>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const darkMode = computed(() => JSON.parse(localStorage.getItem('darkMode')) || false);

const props = defineProps({
  anime: {
    type: Object,
    required: true,
    default: () => ({
      id: null,
      title: "Unknown Title",
      russian_title: "Нет названия",
      uzbek_title: "Sarlavha yo'q",
      english_title: "",
      poster_url: "",
      release_year: "2020",
      type: "MOVIE",
      status: "COMPLETED",
      rating: 0,
      total_episodes: 0,
      total_views: 0,
      is_premium_only: false,
    }),
  },
});

// Title - prioritize based on API structure
const animeTitle = computed(() => {
  return props.anime.title || 
         props.anime.russian_title || 
         props.anime.uzbek_title || 
         props.anime.english_title || 
         "Unknown Title";
});

// Poster
const imageSrc = computed(() => {
  return props.anime.poster_url || "/placeholder-mini.jpg";
});

// Mock watch progress (you can replace this with actual data from localStorage or API)
const watchProgress = computed(() => {
  const stored = localStorage.getItem(`anime_progress_${props.anime.id}`);
  return stored ? parseInt(stored) : 0;
});

// Format views count (e.g., 1500 -> 1.5K, 1500000 -> 1.5M)
function formatViews(views) {
  if (!views) return '0';
  if (views >= 1000000) {
    return (views / 1000000).toFixed(1) + 'M';
  }
  if (views >= 1000) {
    return (views / 1000).toFixed(1) + 'K';
  }
  return views.toString();
}

// Navigate to anime detail
function goToAnime() {
  if (props.anime.id) {
    router.push(`/anime/${props.anime.id}`);
  } else {
    console.warn("❗ Anime ID is missing for navigation.");
  }
}
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>