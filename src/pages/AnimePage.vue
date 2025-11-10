<template>
  <div
    class="min-h-screen w-full transition-colors duration-300"
    :class="darkMode ? 'bg-slate-950 text-white' : 'bg-gray-50 text-slate-900'"
  >
    <!-- Hero Banner Section -->
    <div v-if="!isLoading && anime" class="relative w-full h-[50vh] md:h-[60vh] overflow-hidden">
      <!-- Background Image -->
      <div 
        class="absolute inset-0 bg-cover bg-center scale-110 blur-sm"
        :style="{ backgroundImage: `url(${anime.banner || anime.image})` }"
      ></div>
      <div class="absolute inset-0 bg-gradient-to-t from-black via-black/70 to-black/30"></div>

      <!-- Back Button -->
      <button
        @click="goBack"
        class="absolute top-4 left-4 z-20 flex items-center gap-2 px-4 py-2 rounded-xl bg-black/50 backdrop-blur-md hover:bg-black/70 border border-white/20 transition-all"
      >
        <svg class="h-5 w-5 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
        </svg>
        <span class="font-semibold text-white hidden sm:inline">{{ $t('detail.back') }}</span>
      </button>

      <!-- Hero Content -->
      <div class="absolute inset-0 flex items-end">
        <div class="w-full px-4 pb-6 md:px-8 md:pb-8">
          <div class="max-w-7xl mx-auto">
            <div class="flex gap-4 md:gap-6 items-end">
              <!-- Poster -->
              <img
                :src="anime.image"
                :alt="anime.title"
                class="w-24 h-36 md:w-32 md:h-48 lg:w-40 lg:h-60 object-cover rounded-xl shadow-2xl ring-2 ring-white/30 flex-shrink-0"
              />
              
              <!-- Info -->
              <div class="flex-1 pb-2">
                <h1 class="text-2xl md:text-4xl lg:text-5xl font-black text-white mb-2 md:mb-3 line-clamp-2">
                  {{ anime.title }}
                </h1>
                
                <div class="flex flex-wrap items-center gap-2 mb-3">
                  <span class="flex items-center gap-1 px-2 py-1 rounded-lg bg-yellow-500/20 backdrop-blur-sm border border-yellow-500/30">
                    <svg class="w-4 h-4 text-yellow-400" fill="currentColor" viewBox="0 0 24 24">
                      <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
                    </svg>
                    <span class="text-xs md:text-sm font-bold text-yellow-400">{{ anime.rating }}</span>
                  </span>

                  <span class="px-2 py-1 rounded-lg text-xs md:text-sm font-semibold bg-white/10 backdrop-blur-sm border border-white/20 text-white">
                    {{ anime.type }}
                  </span>

                  <span class="px-2 py-1 rounded-lg text-xs md:text-sm font-semibold bg-white/10 backdrop-blur-sm border border-white/20 text-white">
                    {{ anime.year }}
                  </span>

                  <span 
                    class="px-2 py-1 rounded-lg text-xs md:text-sm font-semibold backdrop-blur-sm border"
                    :class="anime.status === 'Completed' 
                      ? 'bg-blue-500/20 border-blue-500/30 text-blue-400' 
                      : 'bg-green-500/20 border-green-500/30 text-green-400'"
                  >
                    {{ anime.status }}
                  </span>
                </div>

                <p class="text-sm md:text-base text-gray-200 line-clamp-2 hidden md:block">
                  {{ anime.description }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Loading State -->
    <div v-if="isLoading" class="flex flex-col items-center justify-center py-20">
      <div class="w-16 h-16 border-4 border-purple-200 border-t-purple-600 rounded-full animate-spin mb-4"></div>
      <p class="text-xl font-bold">{{ $t('loading') }}...</p>
    </div>

    <!-- Error State -->
    <div v-else-if="!anime || isError" class="flex flex-col items-center justify-center py-20">
      <svg class="w-20 h-20 text-red-500 mb-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
      </svg>
      <p class="text-2xl font-bold">{{ $t('detail.notFound') }}</p>
    </div>

    <!-- Main Content -->
    <div v-else class="max-w-7xl mx-auto px-4 py-6 md:py-8 space-y-6">
      
      <!-- Action Tags -->
      <div class="grid grid-cols-4 gap-2 md:gap-3">
        <button
          v-for="tag in tags"
          :key="tag.name"
          @click="selectTag(tag.name)"
          class="flex flex-col md:flex-row items-center justify-center gap-1 md:gap-2 px-3 py-3 md:py-2.5 rounded-xl font-semibold text-xs md:text-sm transition-all"
          :class="[
            activeTag === tag.name
              ? tag.name === 'watching'
                ? 'bg-gradient-to-br from-blue-500 to-blue-600 text-white shadow-lg'
                : tag.name === 'paused'
                ? 'bg-gradient-to-br from-red-500 to-red-600 text-white shadow-lg'
                : tag.name === 'finished'
                ? 'bg-gradient-to-br from-green-500 to-green-600 text-white shadow-lg'
                : 'bg-gradient-to-br from-pink-500 to-pink-600 text-white shadow-lg'
              : darkMode ? 'bg-slate-800 text-gray-300 hover:bg-slate-700' : 'bg-white text-gray-700 hover:bg-gray-100'
          ]"
        >
          <component :is="tag.icon" class="w-5 h-5 md:w-4 md:h-4" />
          <span class="hidden md:inline">{{ $t(`tag.${tag.name}`) }}</span>
        </button>
      </div>

      <!-- Video Player -->
      <div 
        class="rounded-2xl overflow-hidden shadow-xl"
        :class="darkMode ? 'bg-slate-800' : 'bg-white'"
      >
        <div class="aspect-video bg-black">
          <iframe
            v-if="currentVideoUrl"
            :key="currentVideoUrl"
            class="w-full h-full"
            :src="currentVideoUrl"
            frameborder="0"
            allowfullscreen
            allow="autoplay; fullscreen"
          ></iframe>
          <div v-else class="w-full h-full flex items-center justify-center">
            <div class="text-center">
              <svg class="w-16 h-16 mx-auto text-gray-600 mb-3" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z" />
              </svg>
              <p class="text-gray-500">{{ $t('detail.noVideo') }}</p>
            </div>
          </div>
        </div>

        <!-- Episode Info Bar -->
        <div 
          class="px-4 py-3 border-t flex items-center justify-between"
          :class="darkMode ? 'border-slate-700' : 'border-gray-200'"
        >
          <div>
            <p class="font-bold text-sm md:text-base" :class="darkMode ? 'text-white' : 'text-gray-900'">
              Episode {{ currentEpisode?.episode_number || (selectedEpisode + 1) }}
            </p>
            <p class="text-xs md:text-sm flex items-center gap-2" :class="darkMode ? 'text-gray-400' : 'text-gray-600'">
              <svg class="w-4 h-4" fill="currentColor" viewBox="0 0 24 24">
                <path d="M12 4.5C7 4.5 2.73 7.61 1 12c1.73 4.39 6 7.5 11 7.5s9.27-3.11 11-7.5c-1.73-4.39-6-7.5-11-7.5zM12 17c-2.76 0-5-2.24-5-5s2.24-5 5-5 5 2.24 5 5-2.24 5-5 5zm0-8c-1.66 0-3 1.34-3 3s1.34 3 3 3 3-1.34 3-3-1.34-3-3-3z"/>
              </svg>
              {{ formatViews(currentEpisode?.total_views || 0) }} views
            </p>
          </div>
          <div class="flex gap-2">
            <button
              v-if="selectedEpisode > 0"
              @click="changeEpisode(selectedEpisode - 1)"
              class="p-2 rounded-lg transition-all"
              :class="darkMode ? 'bg-slate-700 hover:bg-slate-600' : 'bg-gray-100 hover:bg-gray-200'"
            >
              <svg class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
              </svg>
            </button>
            <button
              v-if="selectedEpisode < anime.episodes.length - 1"
              @click="changeEpisode(selectedEpisode + 1)"
              class="p-2 rounded-lg transition-all"
              :class="darkMode ? 'bg-slate-700 hover:bg-slate-600' : 'bg-gray-100 hover:bg-gray-200'"
            >
              <svg class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7" />
              </svg>
            </button>
          </div>
        </div>
      </div>

      <!-- Episodes Grid -->
      <div 
        class="rounded-2xl p-4 md:p-6 shadow-xl"
        :class="darkMode ? 'bg-slate-800' : 'bg-white'"
      >
        <div class="flex items-center justify-between mb-4">
          <h2 class="text-xl md:text-2xl font-bold flex items-center gap-2">
            <div class="w-1 h-6 bg-gradient-to-b from-purple-500 to-pink-500 rounded-full"></div>
            Episodes
          </h2>
          <span 
            class="px-3 py-1 rounded-full text-sm font-bold"
            :class="darkMode ? 'bg-slate-700 text-gray-300' : 'bg-gray-200 text-gray-700'"
          >
            {{ anime.episodes.length }} {{ $t('detail.ep') }}
          </span>
        </div>

        <div class="grid grid-cols-4 sm:grid-cols-6 md:grid-cols-8 lg:grid-cols-10 gap-2 md:gap-3">
          <button
            v-for="(ep, i) in anime.episodes"
            :key="ep.id"
            @click="changeEpisode(i)"
            class="aspect-square rounded-xl transition-all flex flex-col items-center justify-center relative"
            :class="[
              selectedEpisode === i
                ? 'bg-gradient-to-br from-purple-500 to-pink-500 text-white shadow-lg scale-105'
                : darkMode ? 'bg-slate-700 hover:bg-slate-600 text-gray-300' : 'bg-gray-100 hover:bg-gray-200 text-gray-700'
            ]"
          >
            <svg 
              v-if="selectedEpisode === i" 
              class="w-6 h-6 mb-1" 
              fill="currentColor" 
              viewBox="0 0 24 24"
            >
              <path d="M8 5v14l11-7z"/>
            </svg>
            <span class="text-base md:text-lg font-bold">{{ ep.episode_number || (i + 1) }}</span>
            <span class="text-xs opacity-75">EP</span>
          </button>
        </div>
      </div>

      <!-- About Section -->
      <div 
        class="rounded-2xl p-4 md:p-6 shadow-xl"
        :class="darkMode ? 'bg-slate-800' : 'bg-white'"
      >
        <h2 class="text-xl md:text-2xl font-bold mb-3 flex items-center gap-2">
          <div class="w-1 h-6 bg-gradient-to-b from-blue-500 to-cyan-500 rounded-full"></div>
          {{ $t('detail.aboutAnime') }}
        </h2>
        <p class="text-sm md:text-base leading-relaxed" :class="darkMode ? 'text-gray-300' : 'text-gray-700'">
          {{ anime.description }}
        </p>
        
        <!-- Genres -->
        <div class="flex flex-wrap gap-2 mt-4">
          <span 
            v-for="(genre, idx) in anime.genre.split(',')" 
            :key="idx"
            class="px-3 py-1 rounded-full text-xs md:text-sm font-medium transition-colors"
            :class="darkMode ? 'bg-slate-700 text-gray-300 hover:bg-slate-600' : 'bg-gray-100 text-gray-700 hover:bg-gray-200'"
          >
            {{ genre.trim() }}
          </span>
        </div>
      </div>

      <!-- Comments Section -->
      <div 
        class="rounded-2xl p-4 md:p-6 shadow-xl"
        :class="darkMode ? 'bg-slate-800' : 'bg-white'"
      >
        <h2 class="text-xl md:text-2xl font-bold mb-4 flex items-center gap-2">
          <div class="w-1 h-6 bg-gradient-to-b from-green-500 to-emerald-500 rounded-full"></div>
          {{ $t('detail.comments') }}
        </h2>

        <!-- Comment Input -->
        <div 
          class="rounded-xl p-3 md:p-4 mb-4 border"
          :class="darkMode ? 'bg-slate-900/50 border-slate-700' : 'bg-gray-50 border-gray-200'"
        >
          <textarea
            v-model="comment"
            :placeholder="$t('detail.commentPlaceholder')"
            class="w-full bg-transparent border-0 resize-none focus:outline-none text-sm md:text-base"
            :class="darkMode ? 'text-white placeholder-gray-500' : 'text-gray-900 placeholder-gray-400'"
            rows="3"
          ></textarea>
          <div class="flex justify-end mt-2">
            <button
              @click="addComment"
              :disabled="isSending"
              class="px-4 md:px-6 py-2 bg-gradient-to-r from-green-500 to-emerald-600 text-white rounded-lg font-semibold shadow-lg transition-all hover:shadow-xl disabled:opacity-50 flex items-center gap-2 text-sm md:text-base"
            >
              <svg class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
              </svg>
              {{ isSending ? $t('detail.sending') : $t('detail.send') }}
            </button>
          </div>
        </div>

        <!-- Comments List -->
        <div class="space-y-3">
          <div
            v-for="(c, i) in comments"
            :key="c.id || i"
            class="rounded-xl p-3 md:p-4 border transition-all"
            :class="darkMode ? 'bg-slate-900/50 border-slate-700' : 'bg-gray-50 border-gray-200'"
          >
            <div class="flex items-start gap-3">
              <div class="w-8 h-8 md:w-10 md:h-10 rounded-full bg-gradient-to-br from-purple-500 to-pink-500 flex items-center justify-center text-white font-bold flex-shrink-0">
                {{ (c.author_name || 'A')[0].toUpperCase() }}
              </div>
              <div class="flex-1 min-w-0">
                <div class="flex items-center gap-2 mb-1">
                  <p class="font-bold text-sm md:text-base" :class="darkMode ? 'text-white' : 'text-gray-900'">
                    {{ c.author_name || 'Anonymous' }}
                  </p>
                  <p class="text-xs" :class="darkMode ? 'text-gray-500' : 'text-gray-500'">
                    {{ formatDate(c.created_at) }}
                  </p>
                </div>
                <p class="text-sm md:text-base break-words" :class="darkMode ? 'text-gray-300' : 'text-gray-700'">
                  {{ c.comment }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <p 
          v-if="!comments.length && !isLoading" 
          class="text-center py-8"
          :class="darkMode ? 'text-gray-500' : 'text-gray-400'"
        >
          {{ $t('detail.noComments') }}
        </p>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch, computed } from "vue";
import { useI18n } from "vue-i18n";
import { useRoute, useRouter } from "vue-router";
import {
  EyeIcon,
  PauseCircleIcon,
  CheckCircleIcon,
  HeartIcon,
} from "lucide-vue-next";

const API_BASE = "https://api.meteordub.uz/api";
const { t } = useI18n();
const route = useRoute();
const router = useRouter();

const anime = ref(null);
const isLoading = ref(true);
const isError = ref(false);
const selectedEpisode = ref(0);
const currentEpisode = ref(null);
const activeTag = ref(null);
const darkMode = ref(false);

const tags = ref([
  { name: "watching", icon: EyeIcon },
  { name: "paused", icon: PauseCircleIcon },
  { name: "finished", icon: CheckCircleIcon },
  { name: "favorite", icon: HeartIcon },
]);

function getLocalAnimeList(tag) {
  const key = `${tag}_anime_ids`;
  try {
    const list = localStorage.getItem(key);
    return list ? JSON.parse(list).map(id => parseInt(id)) : [];
  } catch (e) {
    console.error(`Error parsing localStorage key: ${key}`, e);
    return [];
  }
}

function saveLocalAnimeList(tag, list) {
  const key = `${tag}_anime_ids`;
  localStorage.setItem(key, JSON.stringify(list));
}

function initializeActiveTag() {
  if (!anime.value) return;
  const animeId = parseInt(anime.value.id);
  const allTags = tags.value.map(t => t.name);
  
  for (const tag of allTags) {
    const list = getLocalAnimeList(tag);
    if (list.includes(animeId)) {
      activeTag.value = tag;
      return;
    }
  }
  activeTag.value = null;
}

const selectTag = (tag) => {
  if (!anime.value) return;
  const animeId = parseInt(anime.value.id);
  const currentTag = activeTag.value;

  if (currentTag === tag) {
    const currentList = getLocalAnimeList(currentTag);
    const newList = currentList.filter(id => id !== animeId);
    saveLocalAnimeList(currentTag, newList);
    activeTag.value = null;
  } else {
    if (currentTag) {
      const oldList = getLocalAnimeList(currentTag);
      const newOldList = oldList.filter(id => id !== animeId);
      saveLocalAnimeList(currentTag, newOldList);
    }
    const newList = getLocalAnimeList(tag);
    if (!newList.includes(animeId)) {
      newList.push(animeId);
    }
    saveLocalAnimeList(tag, newList);
    activeTag.value = tag;
  }
};

const currentVideoUrl = computed(() => {
  if (!anime.value || anime.value.episodes.length <= selectedEpisode.value || selectedEpisode.value < 0) {
    return null;
  }
  const episode = anime.value.episodes[selectedEpisode.value];
  
  if (Array.isArray(episode.languages) && episode.languages.length > 0) {
    let videoObject = episode.languages.find(lang => lang.is_default);
    if (!videoObject) {
      videoObject = episode.languages[0];
    }
    return videoObject.video_url || null;
  }
  return episode.video_url || null;
});

const comments = ref([]);
const comment = ref("");
const isSending = ref(false);

// Increment view count
async function incrementEpisodeView(animeId, episodeId) {
  try {
    const res = await fetch(`${API_BASE}/animes/${animeId}/episodes/${episodeId}/`, {
      method: 'GET'
    });
    if (res.ok) {
      const data = await res.json();
      if (data.data && currentEpisode.value) {
        currentEpisode.value.total_views = data.data.total_views;
      }
    }
  } catch (err) {
    console.error("Error incrementing view:", err);
  }
}

async function fetchComments(animeId, episodeId) {
  comments.value = [];
  try {
    const res = await fetch(`${API_BASE}/animes/${animeId}/episodes/${episodeId}/comments/`);
    if (!res.ok) throw new Error(`Failed to fetch comments: ${res.status}`);
    const data = await res.json();
    let loadedComments = Array.isArray(data.results) ? data.results : [];
    if (!loadedComments.length && Array.isArray(data.data)) {
      loadedComments = data.data;
    }
    comments.value = loadedComments.sort((a, b) => 
      new Date(b.created_at).getTime() - new Date(a.created_at).getTime()
    );
  } catch (err) {
    console.error("❌ Error loading comments:", err);
    comments.value = [];
  }
}

async function addComment() {
  const text = comment.value.trim();
  if (!text) {
    alert("⚠️ Comment field is required.");
    return;
  }

  const animeId = anime.value?.id;
  const epId = currentEpisode.value?.id;
  if (!animeId || !epId) {
    console.error("❌ Missing animeId or episodeId for comment");
    return;
  }

  const accessToken = localStorage.getItem("access_token");
  if (!accessToken) {
    alert(t('alert.loginRequired'));
    return;
  }

  isSending.value = true;
  try {
    const res = await fetch(
      `${API_BASE}/animes/${animeId}/episodes/${epId}/comments/`,
      {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          "Authorization": `Bearer ${accessToken}`,
        },
        body: JSON.stringify({ comment: text }),
      }
    );

    const data = await res.json();
    if (!res.ok) {
      const backendError = data?.errors?.comment?.[0] || data?.message || "Failed to post comment.";
      throw new Error(backendError);
    }

    const newComment = data.data || data;
    comments.value.unshift(newComment);
    comment.value = "";
  } catch (err) {
    console.error("❌ Error posting comment:", err);
    alert(`❌ ${err.message}`);
  } finally {
    isSending.value = false;
  }
}

async function changeEpisode(index) {
  if (index < 0 || index >= anime.value.episodes.length) return;
  selectedEpisode.value = index;
  currentEpisode.value = anime.value.episodes[index];
  
  // Increment view and fetch comments
  if (anime.value?.id && currentEpisode.value?.id) {
    await incrementEpisodeView(anime.value.id, currentEpisode.value.id);
    await fetchComments(anime.value.id, currentEpisode.value.id);
  }
}

async function fetchAnimeById(animeId) {
  isLoading.value = true;
  isError.value = false;
  anime.value = null;
  comments.value = [];

  if (!animeId || isNaN(parseInt(animeId))) {
    console.error("Invalid anime ID provided.");
    isError.value = true;
    isLoading.value = false;
    return;
  }

  try {
    const detailUrl = `${API_BASE}/animes/${animeId}/`;
    const detailRes = await fetch(detailUrl);
    if (!detailRes.ok) throw new Error(`HTTP ${detailRes.status}`);

    const detailData = (await detailRes.json()).data;

    const episodeUrl = `${API_BASE}/animes/${animeId}/episodes/`;
    const episodeRes = await fetch(episodeUrl);
    let episodes = [];
    if (episodeRes.ok) {
      const episodeList = await episodeRes.json();
      episodes = Array.isArray(episodeList.data) 
        ? episodeList.data.sort((a, b) => a.episode_number - b.episode_number) 
        : [];
    }

    anime.value = {
      id: detailData.id,
      title: detailData.title || detailData.title_ru || "Title N/A",
      rating: detailData.rating ? detailData.rating.toFixed(1) : "N/A",
      status: detailData.status?.toUpperCase() === "ONGOING" ? "Ongoing" : "Completed",
      type: detailData.type || "N/A",
      year: detailData.release_year || "N/A",
      ageRating: detailData.age_rating || "N/A",
      genre: Array.isArray(detailData.genres) && detailData.genres.length
        ? detailData.genres.map((g) => g.name).join(", ")
        : t("detail.noGenre"),
      image: detailData.poster_url || "/placeholder.jpg",
      banner: detailData.banner_url || detailData.poster_url || "/placeholder.jpg",
      description: detailData.description || t("detail.noDescription"),
      episodes,
    };

    selectedEpisode.value = anime.value.episodes.length > 0 ? 0 : -1;
    currentEpisode.value = anime.value.episodes[0] || null;
    
    if (anime.value.id && currentEpisode.value?.id) {
      await incrementEpisodeView(anime.value.id, currentEpisode.value.id);
      await fetchComments(anime.value.id, currentEpisode.value.id);
    }

    initializeActiveTag();
  } catch (err) {
    console.error("Critical error:", err.message);
    isError.value = true;
    anime.value = null;
  } finally {
    isLoading.value = false;
  }
}

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

function formatDate(dateString) {
  const date = new Date(dateString);
  const now = new Date();
  const diff = now - date;
  const minutes = Math.floor(diff / 60000);
  const hours = Math.floor(diff / 3600000);
  const days = Math.floor(diff / 86400000);

  if (minutes < 1) return 'Just now';
  if (minutes < 60) return `${minutes}m ago`;
  if (hours < 24) return `${hours}h ago`;
  if (days < 7) return `${days}d ago`;
  return date.toLocaleDateString();
}

const goBack = () => router.go(-1);

onMounted(() => {
  const savedMode = JSON.parse(localStorage.getItem("darkMode"));
  darkMode.value = savedMode || false;
  document.documentElement.classList.toggle("dark", darkMode.value);

  const animeId = route.params.id;
  fetchAnimeById(animeId);
});

watch(
  () => route.params.id,
  (newId) => {
    if (newId) fetchAnimeById(newId);
  }
);
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>