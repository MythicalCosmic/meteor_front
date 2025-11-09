<template>
  <div
    class="min-h-screen w-full transition-colors duration-500"
    :class="darkMode ? 'bg-gradient-to-br from-slate-950 via-slate-900 to-slate-950 text-white' : 'bg-gradient-to-br from-gray-50 via-white to-gray-100 text-slate-900'"
  >
    <!-- Hero Banner Section -->
    <div v-if="!isLoading && anime" class="relative w-full h-[400px] md:h-[500px] overflow-hidden">
      <!-- Background Image with Parallax Effect -->
      <div 
        class="absolute inset-0 bg-cover bg-center transform scale-105"
        :style="{ backgroundImage: `url(${anime.image})` }"
      >
        <div class="absolute inset-0 bg-gradient-to-t from-black via-black/60 to-transparent"></div>
        <div class="absolute inset-0 bg-gradient-to-r from-black/90 via-black/50 to-transparent"></div>
      </div>

      <!-- Back Button -->
      <div class="relative z-10 max-w-7xl mx-auto px-4 pt-6">
        <button
          @click="goBack"
          class="group flex items-center gap-2 px-4 py-2 rounded-full backdrop-blur-md bg-white/10 hover:bg-white/20 border border-white/20 transition-all duration-300 hover:scale-105"
        >
          <svg class="h-5 w-5 transition-transform group-hover:-translate-x-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7" />
          </svg>
          <span class="font-semibold text-white">{{ $t('detail.back') }}</span>
        </button>
      </div>

      <!-- Hero Content -->
      <div class="relative z-10 max-w-7xl mx-auto px-4 h-full flex items-end pb-8">
        <div class="flex flex-col md:flex-row gap-6 w-full">
          <!-- Poster Image -->
          <div class="flex-shrink-0">
            <div class="relative group">
              <img
                :src="anime.image"
                :alt="anime.title"
                class="w-48 h-72 md:w-56 md:h-80 object-cover rounded-2xl shadow-2xl ring-4 ring-white/20 transform transition-all duration-500 group-hover:scale-105 group-hover:rotate-1"
              />
              <div class="absolute inset-0 bg-gradient-to-t from-purple-500/20 to-transparent rounded-2xl opacity-0 group-hover:opacity-100 transition-opacity"></div>
            </div>
          </div>

          <!-- Info -->
          <div class="flex-1 space-y-4 pb-4">
            <div>
              <h1 class="text-4xl md:text-5xl lg:text-6xl font-black mb-2 text-white drop-shadow-2xl">
                {{ anime.title }}
              </h1>
              
              <!-- Stats Bar -->
              <div class="flex flex-wrap items-center gap-3 text-sm md:text-base">
                <div class="flex items-center gap-1.5 px-3 py-1.5 rounded-full bg-yellow-500/20 backdrop-blur-sm border border-yellow-500/30">
                  <svg class="w-5 h-5 text-yellow-400" fill="currentColor" viewBox="0 0 24 24">
                    <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z"/>
                  </svg>
                  <span class="font-bold text-yellow-400">{{ anime.rating }}</span>
                </div>

                <span 
                  class="px-3 py-1.5 rounded-full font-semibold backdrop-blur-sm border"
                  :class="anime.status === 'Completed' 
                    ? 'bg-green-500/20 border-green-500/30 text-green-400' 
                    : 'bg-orange-500/20 border-orange-500/30 text-orange-400'"
                >
                  {{ anime.status === 'Completed' ? $t('released') : $t('ongoing') }}
                </span>

                <span class="px-3 py-1.5 rounded-full bg-white/10 backdrop-blur-sm border border-white/20 text-white font-semibold">
                  {{ anime.type }}
                </span>

                <span class="px-3 py-1.5 rounded-full bg-white/10 backdrop-blur-sm border border-white/20 text-white font-semibold">
                  {{ anime.year }}
                </span>

                <span class="px-3 py-1.5 rounded-full bg-purple-500/20 backdrop-blur-sm border border-purple-500/30 text-purple-300 font-semibold">
                  {{ anime.ageRating }}
                </span>
              </div>
            </div>

            <!-- Genres -->
            <div class="flex flex-wrap gap-2">
              <span 
                v-for="(genre, idx) in anime.genre.split(',')" 
                :key="idx"
                class="px-3 py-1 rounded-full text-sm font-medium bg-white/10 backdrop-blur-sm border border-white/20 text-white hover:bg-white/20 transition-colors"
              >
                {{ genre.trim() }}
              </span>
            </div>

            <!-- Description Preview -->
            <p class="text-gray-200 text-base md:text-lg leading-relaxed max-w-3xl line-clamp-2">
              {{ anime.description }}
            </p>

            <!-- Action Buttons -->
            <div class="grid grid-cols-2 md:grid-cols-4 gap-2 max-w-2xl">
              <button
                v-for="tag in tags"
                :key="tag.name"
                @click="selectTag(tag.name)"
                class="flex items-center justify-center gap-2 px-4 py-3 rounded-xl font-semibold transition-all duration-300 hover:scale-105"
                :class="[
                  activeTag === tag.name
                    ? tag.name === 'watching'
                      ? 'bg-gradient-to-r from-blue-500 to-blue-600 text-white shadow-lg shadow-blue-500/50'
                      : tag.name === 'paused'
                      ? 'bg-gradient-to-r from-red-500 to-red-600 text-white shadow-lg shadow-red-500/50'
                      : tag.name === 'finished'
                      ? 'bg-gradient-to-r from-green-500 to-green-600 text-white shadow-lg shadow-green-500/50'
                      : 'bg-gradient-to-r from-pink-500 to-pink-600 text-white shadow-lg shadow-pink-500/50'
                    : 'bg-white/10 backdrop-blur-sm border border-white/20 text-white hover:bg-white/20'
                ]"
              >
                <component :is="tag.icon" class="w-5 h-5" />
                <span class="text-sm">{{ $t(`tag.${tag.name}`) }}</span>
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Loading State -->
    <div v-if="isLoading" class="flex flex-col items-center justify-center py-32">
      <div class="relative w-24 h-24 mb-6">
        <div class="absolute inset-0 border-4 border-purple-200 dark:border-purple-900 rounded-full animate-ping"></div>
        <div class="absolute inset-0 border-4 border-t-purple-600 rounded-full animate-spin"></div>
      </div>
      <p class="text-2xl font-bold">{{ $t('loading') }}...</p>
    </div>

    <!-- Error State -->
    <div v-else-if="!anime || isError" class="flex flex-col items-center justify-center py-32">
      <div class="w-32 h-32 rounded-full flex items-center justify-center mb-6 bg-red-100 dark:bg-red-900/20">
        <svg class="w-16 h-16 text-red-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" />
        </svg>
      </div>
      <p class="text-3xl font-bold mb-2">{{ $t('detail.notFound') }}</p>
    </div>

    <!-- Main Content -->
    <div v-else class="max-w-7xl mx-auto px-4 py-8 space-y-8">
      
      <!-- About Section -->
      <section 
        class="rounded-3xl overflow-hidden shadow-2xl transition-all duration-500"
        :class="darkMode ? 'bg-slate-800/50 backdrop-blur-sm' : 'bg-white/80 backdrop-blur-sm'"
      >
        <div class="p-6 md:p-8">
          <h2 class="text-2xl md:text-3xl font-bold mb-4 flex items-center gap-3">
            <div class="w-1 h-8 bg-gradient-to-b from-purple-500 to-pink-500 rounded-full"></div>
            {{ $t('detail.aboutAnime') }}
          </h2>
          <p class="text-base md:text-lg leading-relaxed" :class="darkMode ? 'text-gray-300' : 'text-gray-700'">
            {{ anime.description }}
          </p>
        </div>
      </section>

      <!-- Episodes Section -->
      <section 
        class="rounded-3xl overflow-hidden shadow-2xl transition-all duration-500"
        :class="darkMode ? 'bg-slate-800/50 backdrop-blur-sm' : 'bg-white/80 backdrop-blur-sm'"
      >
        <div class="p-6 md:p-8">
          <div class="flex items-center justify-between mb-6">
            <h2 class="text-2xl md:text-3xl font-bold flex items-center gap-3">
              <div class="w-1 h-8 bg-gradient-to-b from-blue-500 to-cyan-500 rounded-full"></div>
              Episodes
            </h2>
            <span 
              class="px-4 py-2 rounded-full text-sm font-bold"
              :class="darkMode ? 'bg-slate-700 text-gray-300' : 'bg-gray-200 text-gray-700'"
            >
              {{ anime.episodes.length }} {{ $t('detail.ep') }}
            </span>
          </div>

          <!-- Episodes Grid -->
          <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 xl:grid-cols-8 gap-3">
            <button
              v-for="(ep, i) in anime.episodes"
              :key="ep.id"
              @click="selectedEpisode = i"
              class="group relative aspect-video rounded-xl overflow-hidden transition-all duration-300 hover:scale-105"
              :class="[
                selectedEpisode === i
                  ? 'ring-4 ring-purple-500 shadow-lg shadow-purple-500/50'
                  : 'ring-2 ring-transparent hover:ring-white/30'
              ]"
            >
              <div 
                class="absolute inset-0 transition-all duration-300"
                :class="[
                  selectedEpisode === i
                    ? 'bg-gradient-to-br from-purple-600 to-pink-600'
                    : darkMode ? 'bg-slate-700 group-hover:bg-slate-600' : 'bg-gray-200 group-hover:bg-gray-300'
                ]"
              >
                <div class="absolute inset-0 flex flex-col items-center justify-center">
                  <svg 
                    v-if="selectedEpisode === i" 
                    class="w-8 h-8 mb-1 text-white" 
                    fill="currentColor" 
                    viewBox="0 0 24 24"
                  >
                    <path d="M8 5v14l11-7z"/>
                  </svg>
                  <span 
                    class="text-lg font-bold"
                    :class="selectedEpisode === i ? 'text-white' : darkMode ? 'text-gray-300' : 'text-gray-700'"
                  >
                    {{ ep.episode_number || (i + 1) }}
                  </span>
                  <span 
                    class="text-xs font-medium"
                    :class="selectedEpisode === i ? 'text-white/90' : darkMode ? 'text-gray-400' : 'text-gray-600'"
                  >
                    EP
                  </span>
                </div>
              </div>
            </button>
          </div>
        </div>
      </section>

      <!-- Video Player -->
      <section 
        class="rounded-3xl overflow-hidden shadow-2xl"
        :class="darkMode ? 'bg-slate-800/50 backdrop-blur-sm' : 'bg-white/80 backdrop-blur-sm'"
      >
        <div class="p-4 md:p-6">
          <div class="aspect-video bg-black rounded-2xl overflow-hidden shadow-2xl">
            <iframe
              v-if="currentVideoUrl"
              class="w-full h-full"
              :src="currentVideoUrl"
              frameborder="0"
              allowfullscreen
            ></iframe>
            <div v-else class="w-full h-full flex items-center justify-center">
              <div class="text-center space-y-3">
                <svg class="w-20 h-20 mx-auto text-gray-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 10l4.553-2.276A1 1 0 0121 8.618v6.764a1 1 0 01-1.447.894L15 14M5 18h8a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z" />
                </svg>
                <p class="text-gray-500 text-lg">{{ $t('detail.noVideo') }}</p>
              </div>
            </div>
          </div>
        </div>
      </section>

      <!-- Comments Section -->
      <section 
        class="rounded-3xl overflow-hidden shadow-2xl transition-all duration-500"
        :class="darkMode ? 'bg-slate-800/50 backdrop-blur-sm' : 'bg-white/80 backdrop-blur-sm'"
      >
        <div class="p-6 md:p-8">
          <h2 class="text-2xl md:text-3xl font-bold mb-6 flex items-center gap-3">
            <div class="w-1 h-8 bg-gradient-to-b from-green-500 to-emerald-500 rounded-full"></div>
            {{ $t('detail.comments') }}
          </h2>

          <!-- Comment Input -->
          <div 
            class="rounded-2xl p-4 mb-6 transition-all"
            :class="darkMode ? 'bg-slate-900/50 border border-slate-700' : 'bg-gray-50 border border-gray-200'"
          >
            <textarea
              v-model="comment"
              :placeholder="$t('detail.commentPlaceholder')"
              class="w-full bg-transparent border-0 resize-none focus:outline-none text-base placeholder-gray-500"
              :class="darkMode ? 'text-white' : 'text-gray-900'"
              rows="3"
            ></textarea>
            <div class="flex justify-between items-center mt-3 pt-3 border-t" :class="darkMode ? 'border-slate-700' : 'border-gray-200'">
              <div class="flex gap-2">
                <button 
                  class="p-2 rounded-lg transition-all hover:scale-110"
                  :class="darkMode ? 'hover:bg-slate-800' : 'hover:bg-gray-200'"
                >
                  <span class="font-bold">B</span>
                </button>
                <button 
                  class="p-2 rounded-lg transition-all hover:scale-110"
                  :class="darkMode ? 'hover:bg-slate-800' : 'hover:bg-gray-200'"
                >
                  <span class="italic">I</span>
                </button>
                <button 
                  class="p-2 rounded-lg transition-all hover:scale-110"
                  :class="darkMode ? 'hover:bg-slate-800' : 'hover:bg-gray-200'"
                >
                  <span class="underline">U</span>
                </button>
              </div>
              <button
                @click="addComment"
                :disabled="isSending"
                class="px-6 py-2.5 bg-gradient-to-r from-green-500 to-emerald-600 text-white rounded-xl font-semibold shadow-lg shadow-green-500/30 hover:shadow-green-500/50 transition-all duration-300 hover:scale-105 disabled:opacity-50 disabled:cursor-not-allowed flex items-center gap-2"
              >
                <svg class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8" />
                </svg>
                {{ isSending ? $t('detail.sending') : $t('detail.send') }}
              </button>
            </div>
          </div>

          <!-- Comments List -->
          <div class="space-y-4">
            <transition-group name="comment">
              <div
                v-for="(c, i) in comments"
                :key="c.id || i"
                class="rounded-2xl p-5 transition-all duration-300 hover:scale-[1.02]"
                :class="darkMode ? 'bg-slate-900/50 border border-slate-700' : 'bg-gray-50 border border-gray-200'"
              >
                <div class="flex items-start justify-between mb-3">
                  <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-full bg-gradient-to-br from-purple-500 to-pink-500 flex items-center justify-center text-white font-bold text-lg">
                      {{ (c.author_name || 'A')[0].toUpperCase() }}
                    </div>
                    <div>
                      <p class="font-bold text-lg" :class="darkMode ? 'text-white' : 'text-gray-900'">
                        {{ c.author_name || 'Anonymous' }}
                      </p>
                      <p class="text-xs" :class="darkMode ? 'text-gray-500' : 'text-gray-500'">
                        {{ new Date(c.created_at).toLocaleString() }}
                      </p>
                    </div>
                  </div>
                </div>
                <p class="text-base leading-relaxed" :class="darkMode ? 'text-gray-300' : 'text-gray-700'">
                  {{ c.comment }}
                </p>
              </div>
            </transition-group>
          </div>

          <p 
            v-if="!comments.length && !isLoading" 
            class="text-center py-12"
            :class="darkMode ? 'text-gray-500' : 'text-gray-400'"
          >
            <svg class="w-16 h-16 mx-auto mb-3 opacity-50" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 12h.01M12 12h.01M16 12h.01M21 12c0 4.418-4.03 8-9 8a9.863 9.863 0 01-4.255-.949L3 20l1.395-3.72C3.512 15.042 3 13.574 3 12c0-4.418 4.03-8 9-8s9 3.582 9 8z" />
            </svg>
            {{ $t('detail.noComments') }}
          </p>
        </div>
      </section>

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
  const currentEpisode = anime.value?.episodes[selectedEpisode.value];
  if (!animeId || !currentEpisode?.id) {
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
      `${API_BASE}/animes/${animeId}/episodes/${currentEpisode.id}/comments/`,
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

watch(selectedEpisode, async (newIndex, oldIndex) => {
  if (newIndex === oldIndex || newIndex < 0) return;
  const currentEpisode = anime.value?.episodes[newIndex];
  if (anime.value?.id && currentEpisode?.id) {
    await fetchComments(anime.value.id, currentEpisode.id);
  }
});

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
      title: detailData.title_ru || detailData.title || "Title N/A",
      rating: detailData.rating ? detailData.rating.toFixed(1) : "N/A",
      status: detailData.status?.toUpperCase() === "ONGOING" ? "Ongoing" : "Completed",
      type: detailData.type || "N/A",
      year: detailData.release_year || "N/A",
      ageRating: detailData.age_rating || "N/A",
      genre: Array.isArray(detailData.genres) && detailData.genres.length
        ? detailData.genres.map((g) => g.name).join(", ")
        : t("detail.noGenre"),
      image: detailData.poster_url || "/placeholder.jpg",
      description: detailData.description || t("detail.noDescription"),
      episodes,
    };

    selectedEpisode.value = anime.value.episodes.length > 0 ? 0 : -1;
    
    const initialEpisode = anime.value.episodes[selectedEpisode.value];
    if (anime.value.id && initialEpisode?.id && selectedEpisode.value !== -1) {
      await fetchComments(anime.value.id, initialEpisode.id);
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

.comment-enter-active,
.comment-leave-active {
  transition: all 0.4s ease;
}

.comment-enter-from {
  opacity: 0;
  transform: translateY(20px) scale(0.95);
}

.comment-leave-to {
  opacity: 0;
  transform: translateX(-20px) scale(0.95);
}
</style>