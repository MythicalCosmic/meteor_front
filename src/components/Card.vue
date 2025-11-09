<template>
  <div
    @click="goToAnime"
    class="group relative rounded-2xl overflow-hidden cursor-pointer transition-all duration-500 hover:scale-105"
  >
    <!-- Main Card Container -->
    <div
      class="relative aspect-[2/3] overflow-hidden rounded-2xl shadow-lg transition-all duration-500"
      :class="darkMode 
        ? 'bg-slate-800 shadow-slate-900/50 group-hover:shadow-purple-500/50 group-hover:shadow-2xl' 
        : 'bg-gray-100 shadow-gray-300/50 group-hover:shadow-purple-400/50 group-hover:shadow-2xl'"
    >
      <!-- Image -->
      <img
        :src="imageSrc"
        :alt="animeTitle"
        class="w-full h-full object-cover transition-transform duration-700 group-hover:scale-110"
        loading="lazy"
      />

      <!-- Gradient Overlay -->
      <div class="absolute inset-0 bg-gradient-to-t from-black via-black/50 to-transparent opacity-60 group-hover:opacity-80 transition-opacity duration-500"></div>

      <!-- Top Badges -->
      <div class="absolute top-3 left-3 flex flex-col gap-2 z-10">
        <!-- Rating Badge -->
        <div
          v-if="anime.rating"
          class="flex items-center gap-1 px-3 py-1.5 bg-gradient-to-r from-yellow-400 to-amber-500 rounded-full shadow-lg backdrop-blur-sm"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5 text-yellow-900" fill="currentColor" viewBox="0 0 24 24">
            <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
          </svg>
          <span class="text-xs font-bold text-yellow-900">{{ anime.rating.toFixed(1) }}</span>
        </div>

        <!-- Status Badge -->
        <div
          v-if="anime.status"
          class="px-3 py-1 rounded-full text-xs font-semibold shadow-md backdrop-blur-sm"
          :class="anime.status === 'ongoing' 
            ? 'bg-green-500/90 text-white'
            : anime.status === 'COMPLETED'
            ? 'bg-blue-500/90 text-white'
            : 'bg-gray-500/90 text-white'"
        >
          <span class="flex items-center gap-1">
            <span v-if="anime.status === 'ongoing'" class="w-1.5 h-1.5 bg-white rounded-full animate-pulse"></span>
            {{ anime.status === 'ongoing' ? 'Ongoing' : anime.status === 'COMPLETED' ? 'Completed' : anime.status }}
          </span>
        </div>
      </div>

      <!-- Type Badge (Top Right) -->
      <div
        v-if="anime.type"
        class="absolute top-3 right-3 px-3 py-1 rounded-full text-xs font-semibold backdrop-blur-md bg-white/20 text-white border border-white/30 shadow-lg z-10"
      >
        {{ anime.type }}
      </div>

      <!-- Play Overlay (appears on hover) -->
      <div class="absolute inset-0 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-all duration-500 z-10">
        <div class="w-16 h-16 rounded-full bg-white/30 backdrop-blur-md flex items-center justify-center transform scale-75 group-hover:scale-100 transition-transform duration-500 shadow-2xl border-2 border-white/50">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-white ml-1" fill="currentColor" viewBox="0 0 24 24">
            <path d="M8 5v14l11-7z"/>
          </svg>
        </div>
      </div>

      <!-- Bottom Info -->
      <div class="absolute bottom-0 left-0 right-0 p-4 transform translate-y-2 group-hover:translate-y-0 transition-transform duration-500 z-10">
        <!-- Title -->
        <h3 class="text-white font-bold text-base sm:text-lg leading-tight mb-2 line-clamp-2 drop-shadow-lg">
          {{ animeTitle }}
        </h3>

        <!-- Metadata -->
        <div class="flex items-center justify-between text-xs text-white/90 opacity-0 group-hover:opacity-100 transition-opacity duration-500">
          <span class="flex items-center gap-1">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z" />
            </svg>
            {{ anime.release_year || anime.year || 'N/A' }}
          </span>
          
          <span v-if="anime.total_episodes" class="flex items-center gap-1">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-3.5 w-3.5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 4v16M17 4v16M3 8h4m10 0h4M3 12h18M3 16h4m10 0h4M4 20h16a1 1 0 001-1V5a1 1 0 00-1-1H4a1 1 0 00-1 1v14a1 1 0 001 1z" />
            </svg>
            {{ anime.total_episodes }} {{ anime.total_episodes === 1 ? 'ep' : 'eps' }}
          </span>
        </div>

        <!-- Progress Bar (if watching) -->
        <div v-if="watchProgress > 0" class="mt-2 opacity-0 group-hover:opacity-100 transition-opacity duration-500">
          <div class="w-full h-1 bg-white/20 rounded-full overflow-hidden backdrop-blur-sm">
            <div 
              class="h-full bg-gradient-to-r from-purple-500 to-pink-500 rounded-full transition-all duration-500"
              :style="{ width: `${watchProgress}%` }"
            ></div>
          </div>
          <p class="text-xs text-white/80 mt-1">{{ watchProgress }}% watched</p>
        </div>
      </div>

      <!-- Shimmer Effect on Hover -->
      <div class="absolute inset-0 opacity-0 group-hover:opacity-100 transition-opacity duration-500 pointer-events-none">
        <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/10 to-transparent -translate-x-full group-hover:translate-x-full transition-transform duration-1000"></div>
      </div>
    </div>

    <!-- Premium Badge (if premium) -->
    <div
      v-if="anime.is_premium_only"
      class="absolute -top-2 -right-2 z-20"
    >
      <div class="relative">
        <div class="w-10 h-10 bg-gradient-to-br from-yellow-400 to-amber-500 rounded-full flex items-center justify-center shadow-lg shadow-yellow-500/50 border-2 border-white animate-pulse">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-yellow-900" fill="currentColor" viewBox="0 0 24 24">
            <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
          </svg>
        </div>
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
      title_ru: "Нет названия",
      title_uz: "Sarlavha yo'q",
      poster_url: "",
      year: "2020",
      type: "series",
      status: "released",
      rating: 0,
      total_episodes: 0,
      is_premium_only: false,
    }),
  },
});

// Title
const animeTitle = computed(
  () => props.anime.title_ru || props.anime.title_uz || props.anime.title || "Unknown Title"
);

// Poster
const imageSrc = computed(() => {
  return props.anime.poster_url || "/placeholder-mini.jpg";
});

// Mock watch progress (you can replace this with actual data from localStorage or API)
const watchProgress = computed(() => {
  // Example: Get from localStorage
  const stored = localStorage.getItem(`anime_progress_${props.anime.id}`);
  return stored ? parseInt(stored) : 0;
});

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