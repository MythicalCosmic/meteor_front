<template>
  <div
    class="min-h-screen w-full transition-colors duration-500"
    :class="darkMode ? 'bg-gradient-to-br from-slate-950 via-slate-900 to-slate-950 text-white' : 'bg-gradient-to-br from-gray-50 via-white to-gray-100 text-slate-900'"
  >
    <div class="max-w-7xl mx-auto px-4">
      <!-- HERO SLIDER -->
      <section
        v-if="showSlider && slides.length"
        class="relative rounded-3xl overflow-hidden mt-6 mb-8 shadow-2xl group"
        :style="{
          backgroundImage: `url(${slides[currentIndex].hero})`,
          minHeight: windowWidth < 640 ? '400px' : windowWidth < 1024 ? '500px' : '600px',
        }"
      >
        <!-- Gradient Overlay -->
        <div class="absolute inset-0 bg-gradient-to-r from-black/90 via-black/60 to-transparent"></div>
        <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-transparent to-transparent"></div>

        <!-- Content -->
        <div class="relative z-10 h-full flex items-center px-6 sm:px-10 md:px-16 py-16">
          <div class="max-w-2xl space-y-6">
            <transition name="fade-up" mode="out-in">
              <div :key="slides[currentIndex].id">
                <!-- Badge -->
                <div class="inline-flex items-center gap-2 px-4 py-2 rounded-full bg-gradient-to-r from-red-500 to-pink-500 text-white text-sm font-semibold mb-4 shadow-lg">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="currentColor" viewBox="0 0 24 24">
                    <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
                  </svg>
                  <span>{{ slides[currentIndex].label }}</span>
                </div>

                <!-- Title -->
                <h1 class="text-4xl sm:text-5xl md:text-6xl lg:text-7xl font-black leading-tight text-white drop-shadow-2xl mb-4">
                  {{ slides[currentIndex].title }}
                </h1>

                <!-- Metadata -->
                <div class="flex items-center gap-4 text-white/90 text-sm sm:text-base mb-4">
                  <span class="flex items-center gap-1">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-yellow-400" fill="currentColor" viewBox="0 0 24 24">
                      <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
                    </svg>
                    8.5
                  </span>
                  <span>•</span>
                  <span>{{ slides[currentIndex].subtitle }}</span>
                </div>

                <!-- Description -->
                <p class="text-base sm:text-lg text-gray-200 leading-relaxed line-clamp-3 mb-6">
                  {{ slides[currentIndex].desc }}
                </p>

                <!-- Action Buttons -->
                <div class="flex flex-wrap gap-3">
                  <button
                    :disabled="!slides[currentIndex]?.id"
                    @click="navigateToAnime(slides[currentIndex]?.id)"
                    class="group px-8 py-4 bg-gradient-to-r from-red-500 to-pink-600 hover:from-red-600 hover:to-pink-700 rounded-2xl text-white font-bold text-lg shadow-lg shadow-red-500/50 hover:shadow-red-500/70 hover:scale-105 transition-all duration-300 disabled:opacity-50 flex items-center gap-2"
                  >
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="currentColor" viewBox="0 0 24 24">
                      <path d="M8 5v14l11-7z"/>
                    </svg>
                    <span>{{ $t('watch') }}</span>
                  </button>
                  <button
                    :disabled="!slides[currentIndex]?.id"
                    @click="navigateToAnimeDetails(slides[currentIndex]?.id)"
                    class="px-8 py-4 bg-white/10 hover:bg-white/20 backdrop-blur-md border-2 border-white/30 hover:border-white/50 rounded-2xl text-white font-bold text-lg transition-all duration-300 disabled:opacity-50"
                  >
                    {{ $t('details') }}
                  </button>
                </div>
              </div>
            </transition>
          </div>
        </div>

        <!-- Navigation Dots -->
        <div class="absolute bottom-6 left-0 right-0 flex justify-center gap-2 z-20">
          <button
            v-for="(slide, i) in slides"
            :key="i"
            @click="setSlide(i)"
            class="transition-all duration-300"
            :class="i === currentIndex
              ? 'w-10 h-3 bg-white rounded-full'
              : 'w-3 h-3 bg-white/40 hover:bg-white/60 rounded-full'"
          ></button>
        </div>

        <!-- Side Mini Poster (Desktop) -->
        <div class="hidden lg:block absolute right-12 top-1/2 -translate-y-1/2 z-20">
          <transition name="fade-scale" mode="out-in">
            <div
              :key="slides[currentIndex].id"
              class="relative w-64 h-96 rounded-2xl overflow-hidden shadow-2xl group hover:scale-105 transition-transform duration-500"
            >
              <img
                :src="slides[currentIndex].mini"
                :alt="slides[currentIndex].title"
                class="w-full h-full object-cover"
              />
              <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/40 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
            </div>
          </transition>
        </div>
      </section>

      <!-- TRENDING THIS WEEK SECTION -->
      <section class="mb-12">
        <div class="flex items-center justify-between mb-6">
          <div class="flex items-center gap-3">
            <div class="w-1 h-8 bg-gradient-to-b from-red-500 to-pink-500 rounded-full"></div>
            <h2 class="text-3xl font-bold">
              🔥 Trending This Week
            </h2>
          </div>
          <button
            @click="router.push({ query: { ordering: '-rating' } })"
            class="px-4 py-2 rounded-xl text-sm font-semibold transition-all duration-300 hover:scale-105"
            :class="darkMode ? 'bg-slate-800 hover:bg-slate-700 text-white' : 'bg-white hover:bg-gray-50 text-gray-900 shadow-md'"
          >
            View All →
          </button>
        </div>
        
        <div v-if="isLoadingTrending" class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-5 gap-4">
          <div v-for="n in 5" :key="n" class="animate-pulse">
            <div class="aspect-[2/3] rounded-2xl" :class="darkMode ? 'bg-slate-800' : 'bg-gray-200'"></div>
          </div>
        </div>

        <div v-else class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-5 gap-4">
          <div
            v-for="(anime, idx) in trendingAnimes"
            :key="anime.id"
            @click="navigateToAnime(anime.id)"
            class="group relative cursor-pointer"
          >
            <div class="relative aspect-[2/3] rounded-2xl overflow-hidden shadow-lg transition-all duration-300 group-hover:scale-105 group-hover:shadow-2xl">
              <img
                :src="anime.poster_url || '/placeholder.jpg'"
                :alt="anime.title"
                class="w-full h-full object-cover"
              />
              <!-- Gradient Overlay -->
              <div class="absolute inset-0 bg-gradient-to-t from-black via-transparent to-transparent opacity-60 group-hover:opacity-80 transition-opacity"></div>
              
              <!-- Rank Badge -->
              <div class="absolute top-3 left-3 w-10 h-10 rounded-full bg-gradient-to-r from-yellow-400 to-orange-500 flex items-center justify-center font-black text-white text-lg shadow-lg">
                {{ idx + 1 }}
              </div>

              <!-- Info Overlay -->
              <div class="absolute bottom-0 left-0 right-0 p-4 transform translate-y-2 group-hover:translate-y-0 transition-transform">
                <h3 class="text-white font-bold text-sm mb-1 line-clamp-2">
                  {{ anime.title_ru || anime.title || 'Unknown' }}
                </h3>
                <div class="flex items-center gap-2 text-xs text-white/90">
                  <span class="flex items-center gap-1">
                    <svg class="w-4 h-4 text-yellow-400" fill="currentColor" viewBox="0 0 24 24">
                      <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
                    </svg>
                    {{ anime.rating ? anime.rating.toFixed(1) : 'N/A' }}
                  </span>
                  <span>•</span>
                  <span>{{ anime.release_year || '—' }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- BROWSE BY GENRE SECTION -->
      <section class="mb-12">
        <div class="flex items-center justify-between mb-6">
          <div class="flex items-center gap-3">
            <div class="w-1 h-8 bg-gradient-to-b from-purple-500 to-pink-500 rounded-full"></div>
            <h2 class="text-3xl font-bold">
              Browse by Genre
            </h2>
          </div>
        </div>

        <div v-if="isLoadingGenres" class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-4">
          <div v-for="n in 12" :key="n" class="animate-pulse">
            <div class="h-24 rounded-2xl" :class="darkMode ? 'bg-slate-800' : 'bg-gray-200'"></div>
          </div>
        </div>

        <div v-else class="grid grid-cols-2 sm:grid-cols-3 lg:grid-cols-6 gap-4">
          <button
            v-for="genre in genres"
            :key="genre.id"
            @click="filterByGenre(genre.id)"
            class="group relative overflow-hidden rounded-2xl p-6 transition-all duration-300 hover:scale-105 border"
            :class="darkMode
              ? 'bg-gradient-to-br from-slate-800 to-slate-900 hover:from-slate-700 hover:to-slate-800 border-slate-700'
              : 'bg-gradient-to-br from-white to-gray-50 hover:from-gray-50 hover:to-gray-100 border-gray-200 shadow-md hover:shadow-xl'"
          >
            <div class="absolute inset-0 bg-gradient-to-br from-purple-500/10 to-pink-500/10 opacity-0 group-hover:opacity-100 transition-opacity"></div>
            <div class="relative flex flex-col items-center gap-3">
              <div class="text-4xl">{{ getGenreIcon(genre.name) }}</div>
              <span class="text-sm font-semibold text-center">{{ genre.name }}</span>
              <span class="text-xs opacity-60">{{ genre.anime_count || 0 }} titles</span>
            </div>
          </button>
        </div>
      </section>

      <!-- RECENTLY ADDED -->
      <section class="mb-12">
        <div class="flex items-center justify-between mb-6">
          <div class="flex items-center gap-3">
            <div class="w-1 h-8 bg-gradient-to-b from-green-500 to-emerald-500 rounded-full"></div>
            <h2 class="text-3xl font-bold">
              ⚡ Recently Added
            </h2>
          </div>
        </div>

        <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-4">
          <Card v-for="anime in recentAnimes.slice(0, 12)" :key="anime.id" :anime="anime" />
        </div>
      </section>

      <!-- MAIN ANIME LIST -->
      <section
        class="mb-12 rounded-3xl overflow-hidden transition-colors duration-500 shadow-xl"
        :class="darkMode ? 'bg-slate-800/50 backdrop-blur-sm' : 'bg-white/80 backdrop-blur-sm'"
      >
        <div class="px-6 pt-8 pb-4 flex items-center justify-between">
          <div class="flex items-center gap-3">
            <div class="w-1 h-8 bg-gradient-to-b from-blue-500 to-cyan-500 rounded-full"></div>
            <h2 class="text-3xl font-bold">
              {{
                currentSearchQuery || currentGenres.length || isFiltered
                  ? $t('searchResults')
                  : 'All Anime'
              }}
            </h2>
          </div>
          <div
            v-if="animeList.length"
            class="px-4 py-2 rounded-full text-sm font-semibold"
            :class="darkMode ? 'bg-slate-700 text-gray-300' : 'bg-gray-200 text-gray-700'"
          >
            {{ animeList.length }} {{ $t('animes') }}
          </div>
        </div>

        <!-- Loading State -->
        <div v-if="isLoadingAnimes" class="flex flex-col items-center justify-center py-20">
          <div class="relative w-20 h-20 mb-4">
            <div class="absolute inset-0 border-4 border-purple-200 rounded-full animate-ping"></div>
            <div class="absolute inset-0 border-4 border-t-purple-600 rounded-full animate-spin"></div>
          </div>
          <p class="text-lg font-medium" :class="darkMode ? 'text-gray-400' : 'text-gray-600'">
            {{ $t('loading') }}...
          </p>
        </div>

        <!-- Empty State -->
        <div v-else-if="animeList.length === 0" class="flex flex-col items-center justify-center py-20">
          <div class="w-32 h-32 rounded-full flex items-center justify-center mb-6"
            :class="darkMode ? 'bg-slate-700' : 'bg-gray-100'">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-16 h-16" :class="darkMode ? 'text-gray-500' : 'text-gray-400'" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.172 16.172a4 4 0 015.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
          </div>
          <p class="text-2xl font-bold mb-2">{{ $t('noAnimesFound') }}</p>
          <p class="text-gray-500 mb-6">{{ $t('tryDifferentSearch') }}</p>
        </div>

        <!-- Anime Grid -->
        <div
          v-else
          class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-4 sm:gap-6 p-6"
        >
          <Card v-for="anime in animeList" :key="anime.id" :anime="anime" />
        </div>

        <!-- Pagination -->
        <div
          v-if="totalPages > 1"
          class="flex justify-center items-center gap-2 py-8 flex-wrap"
        >
          <button
            class="px-4 py-2 rounded-xl font-medium transition-all duration-300 flex items-center gap-2"
            :class="animeCurrentPage === 1
              ? darkMode ? 'bg-slate-700 text-gray-500 cursor-not-allowed' : 'bg-gray-200 text-gray-400 cursor-not-allowed'
              : darkMode ? 'bg-slate-700 text-white hover:bg-purple-600 hover:scale-105' : 'bg-white text-gray-800 hover:bg-purple-500 hover:text-white shadow-md hover:shadow-lg hover:scale-105'"
            :disabled="animeCurrentPage === 1"
            @click="changePage(animeCurrentPage - 1)"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
            </svg>
            <span class="hidden sm:inline">{{ $t('prev') }}</span>
          </button>

          <button
            v-for="page in visiblePages"
            :key="page"
            @click="changePage(page)"
            :class="[
              'px-4 py-2 rounded-xl font-semibold transition-all duration-300',
              page === animeCurrentPage
                ? 'bg-gradient-to-r from-purple-500 to-pink-600 text-white shadow-lg shadow-purple-500/50 scale-110'
                : darkMode
                  ? 'bg-slate-700 text-gray-300 hover:bg-slate-600 hover:scale-105'
                  : 'bg-white text-gray-700 hover:bg-gray-100 shadow-md hover:shadow-lg hover:scale-105'
            ]"
          >
            {{ page }}
          </button>

          <button
            class="px-4 py-2 rounded-xl font-medium transition-all duration-300 flex items-center gap-2"
            :class="animeCurrentPage === totalPages
              ? darkMode ? 'bg-slate-700 text-gray-500 cursor-not-allowed' : 'bg-gray-200 text-gray-400 cursor-not-allowed'
              : darkMode ? 'bg-slate-700 text-white hover:bg-purple-600 hover:scale-105' : 'bg-white text-gray-800 hover:bg-purple-500 hover:text-white shadow-md hover:shadow-lg hover:scale-105'"
            :disabled="animeCurrentPage === totalPages"
            @click="changePage(animeCurrentPage + 1)"
          >
            <span class="hidden sm:inline">{{ $t('next') }}</span>
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
            </svg>
          </button>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch, computed } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { useI18n } from 'vue-i18n';
import Card from '../components/Card.vue';

const { t } = useI18n();
const router = useRouter();
const route = useRoute();

const API_BASE = 'https://api.meteordub.uz/api';

// STATE
const currentIndex = ref(0);
const slides = ref([]);
const animeList = ref([]);
const trendingAnimes = ref([]);
const recentAnimes = ref([]);
const genres = ref([]);
const animeNextPage = ref(null);
const animeCurrentPage = ref(1);
const isLoadingAnimes = ref(false);
const isLoadingSlider = ref(true);
const isLoadingTrending = ref(true);
const isLoadingGenres = ref(true);
const darkMode = ref(JSON.parse(localStorage.getItem('darkMode')) || false);
const totalPages = ref(1);

// FILTERS / SEARCH
const currentSearchQuery = ref(route.query.search || '');
const currentGenres = ref([]);
const currentMainFilter = ref({
  search: route.query.search || '',
  type: route.query.type || '',
  status: route.query.status || '',
  season: route.query.season || '',
  yearFrom: route.query.release_year_gte || '',
  yearTo: route.query.release_year_lte || '',
  sort: route.query.ordering || '',
});
const isFiltered = computed(() => Object.values(currentMainFilter.value).some(v => v));
const showSlider = computed(() => !route.query.search && !isFiltered.value && !currentGenres.value.length);

// UI
const windowWidth = ref(window.innerWidth);
const interval = ref(null);

// Genre icon mapping
function getGenreIcon(genreName) {
  const icons = {
    'Action': '⚔️',
    'Adventure': '🗺️',
    'Comedy': '😂',
    'Drama': '🎭',
    'Fantasy': '🔮',
    'Horror': '👻',
    'Mystery': '🔍',
    'Romance': '💕',
    'Sci-Fi': '🚀',
    'Slice of Life': '🌸',
    'Sports': '⚽',
    'Supernatural': '✨',
    'Thriller': '😱',
    'Mecha': '🤖',
    'Music': '🎵',
    'Psychological': '🧠'
  };
  return icons[genreName] || '📺';
}

function filterByGenre(genreId) {
  router.push({ name: 'Home', query: { genres: genreId } });
}

function buildQueryString(page = 1) {
  const params = new URLSearchParams();
  if (route.query.search) params.append('search', route.query.search);
  if (route.query.genres) params.append('genres', route.query.genres);
  if (route.query.type) params.append('type', route.query.type);
  if (route.query.status) params.append('status', route.query.status);
  if (route.query.season) params.append('season', route.query.season);
  if (route.query.release_year) params.append('release_year', route.query.release_year);
  if (route.query.release_year_gte) params.append('release_year_gte', route.query.release_year_gte);
  if (route.query.release_year_lte) params.append('release_year_lte', route.query.release_year_lte);
  if (route.query.ordering) params.append('ordering', route.query.ordering);
  params.append('page', page);
  return params.toString();
}

async function fetchGenres() {
  isLoadingGenres.value = true;
  try {
    const res = await fetch(`${API_BASE}/genres/`);
    const data = await res.json();
    genres.value = data.data || [];
  } catch (err) {
    console.error('❌ Error loading genres:', err);
  } finally {
    isLoadingGenres.value = false;
  }
}

async function fetchTrendingAnimes() {
  isLoadingTrending.value = true;
  try {
    const res = await fetch(`${API_BASE}/animes/?ordering=-rating&limit=10`);
    const data = await res.json();
    trendingAnimes.value = (data.data || []).slice(0, 5);
  } catch (err) {
    console.error('❌ Error loading trending:', err);
  } finally {
    isLoadingTrending.value = false;
  }
}

async function fetchAnimes(append = false) {
  isLoadingAnimes.value = true;
  const pageToLoad = append ? animeCurrentPage.value + 1 : animeCurrentPage.value;
  const url = `${API_BASE}/animes/?${buildQueryString(pageToLoad)}`;

  try {
    const res = await fetch(url);
    const data = await res.json();
    const animes = data.data || [];

    animeList.value = animes;
    recentAnimes.value = animes;
    animeCurrentPage.value = pageToLoad;

    if (data.meta?.count) {
      totalPages.value = Math.ceil(data.meta.count / 12);
    } else {
      totalPages.value = 1;
    }

    animeNextPage.value = data.meta?.next || null;
  } catch (err) {
    console.error('❌ Error loading animes:', err);
  } finally {
    isLoadingAnimes.value = false;
  }
}

function changePage(page) {
  if (page < 1 || page > totalPages.value) return;
  animeCurrentPage.value = page;
  fetchAnimes();
  window.scrollTo({ top: 0, behavior: 'smooth' });
}

async function fetchAnimesForSlider() {
  isLoadingSlider.value = true;
  try {
    const res = await fetch(`${API_BASE}/animes/?limit=20&ordering=-rating`);
    const data = await res.json();
    const all = data.data || [];
    const randomSlides = all.sort(() => 0.5 - Math.random()).slice(0, 5);
    slides.value = randomSlides.map(a => ({
      id: a.id,
      hero: a.banner_url || a.poster_url || '/placeholder-hero.jpg',
      mini: a.poster_url || '/placeholder-mini.jpg',
      title: a.russian_title || a.uzbek_title || a.title || 'No title',
      desc: a.description?.slice(0, 200) + '...' || 'No description available',
      label: a.status === 'ongoing' ? t('ongoing') : t('released'),
      subtitle: `${a.release_year || '—'} • ${a.type || '—'}`,
    }));
    startSlider();
  } catch (err) {
    console.error('❌ Slider fetch failed:', err);
  } finally {
    isLoadingSlider.value = false;
  }
}

watch(
  () => route.query,
  (newQuery) => {
    currentSearchQuery.value = newQuery.search || '';
    currentGenres.value = newQuery.genres ? newQuery.genres.split(',') : [];

    currentMainFilter.value = {
      search: newQuery.search || '',
      type: newQuery.type || '',
      status: newQuery.status || '',
      season: newQuery.season || '',
      yearFrom: newQuery.release_year_gte || '',
      yearTo: newQuery.release_year_lte || '',
      sort: newQuery.ordering || '',
    };

    fetchAnimes();
  },
  { immediate: true }
);

function startSlider() {
  if (interval.value) clearInterval(interval.value);
  if (slides.value.length > 1) {
    interval.value = setInterval(() => {
      currentIndex.value = (currentIndex.value + 1) % slides.value.length;
    }, 5000);
  }
}

function setSlide(i) {
  currentIndex.value = i;
  clearInterval(interval.value);
  startSlider();
}

function navigateToAnime(id) {
  router.push(`/anime/${id}`);
}

function navigateToAnimeDetails(id) {
  router.push(`/anime/${id}`);
}

onMounted(() => {
  document.documentElement.classList.toggle('dark', darkMode.value);
  window.addEventListener('resize', () => (windowWidth.value = window.innerWidth));
  fetchAnimesForSlider();
  fetchGenres();
  fetchTrendingAnimes();
});

onUnmounted(() => {
  clearInterval(interval.value);
  window.removeEventListener('resize', () => (windowWidth.value = window.innerWidth));
});

const visiblePages = computed(() => {
  const total = totalPages.value;
  const current = animeCurrentPage.value;
  const delta = 2;
  const pages = [];

  for (let i = Math.max(1, current - delta); i <= Math.min(total, current + delta); i++) {
    pages.push(i);
  }
  return pages;
});
</script>

<style scoped>
.fade-up-enter-active, .fade-up-leave-active {
  transition: all 0.5s ease;
}
.fade-up-enter-from {
  opacity: 0;
  transform: translateY(20px);
}
.fade-up-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}

.fade-scale-enter-active, .fade-scale-leave-active {
  transition: all 0.5s ease;
}
.fade-scale-enter-from, .fade-scale-leave-to {
  opacity: 0;
  transform: scale(0.9);
}

.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>