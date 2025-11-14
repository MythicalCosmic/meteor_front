<template>
  <div
    class="min-h-screen transition-colors duration-500"
    :class="darkMode ? 'bg-gradient-to-br from-slate-950 via-indigo-950 to-slate-950 text-white' : 'bg-gradient-to-br from-blue-50 via-indigo-50 to-purple-50 text-slate-900'"
  >
    <!-- Animated Background Particles -->
    <div class="fixed inset-0 overflow-hidden pointer-events-none opacity-30">
      <div class="absolute top-1/4 left-1/4 w-96 h-96 bg-blue-500/20 rounded-full blur-3xl animate-pulse"></div>
      <div class="absolute bottom-1/4 right-1/4 w-96 h-96 bg-purple-500/20 rounded-full blur-3xl animate-pulse" style="animation-delay: 2s"></div>
      <div class="absolute top-1/2 left-1/2 w-96 h-96 bg-pink-500/20 rounded-full blur-3xl animate-pulse" style="animation-delay: 4s"></div>
    </div>

    <!-- Header with Banner -->
    <div class="relative w-full">
      <!-- Banner Image with Stunning Effects -->
      <div
        class="w-full h-72 md:h-96 relative overflow-hidden"
        :style="`background-image: url(${background}); background-size: cover; background-position: center;`"
      >
        <!-- Animated gradient overlay -->
        <div class="absolute inset-0 bg-gradient-to-br from-purple-900/60 via-blue-900/50 to-pink-900/60 animate-gradient"></div>
        <div
          class="absolute inset-0 backdrop-blur-sm transition-all duration-500"
          :class="darkMode ? 'bg-gradient-to-b from-black/70 via-purple-900/30 to-slate-950' : 'bg-gradient-to-b from-black/40 via-blue-900/20 to-white'"
        ></div>
        
        <!-- Floating particles effect -->
        <div class="absolute inset-0 opacity-40">
          <div v-for="i in 12" :key="i" 
               class="absolute w-1 h-1 bg-white rounded-full animate-float"
               :style="{
                 top: `${Math.random() * 100}%`,
                 left: `${Math.random() * 100}%`,
                 animationDelay: `${Math.random() * 5}s`,
                 animationDuration: `${3 + Math.random() * 4}s`
               }"></div>
        </div>
        
        <!-- Back Button (Top Left) -->
        <button
          @click="goBack"
          class="absolute top-6 left-6 p-3 rounded-2xl backdrop-blur-xl transition-all hover:scale-110 shadow-2xl z-10 group"
          :class="darkMode ? 'bg-white/10 hover:bg-white/20 text-white border border-white/20' : 'bg-white/30 hover:bg-white/50 text-white border border-white/40'"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 transition-transform group-hover:-translate-x-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M10 19l-7-7m0 0l7-7m-7 7h18" />
          </svg>
        </button>

        <!-- Decorative Elements -->
        <div class="absolute top-10 right-10 w-20 h-20 border-2 border-white/20 rounded-full animate-spin-slow"></div>
        <div class="absolute bottom-20 left-20 w-16 h-16 border-2 border-white/10 rounded-lg rotate-45 animate-pulse"></div>
      </div>

      <!-- Profile Card Container -->
      <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="relative -mt-32 md:-mt-40">
          <!-- Profile Card with Glass Effect -->
          <div
            class="rounded-3xl shadow-2xl p-6 md:p-10 backdrop-blur-2xl transition-all duration-500 border-2 relative overflow-hidden group"
            :class="darkMode 
              ? 'bg-slate-900/60 border-slate-700/50 hover:border-blue-500/50' 
              : 'bg-white/80 border-white/50 hover:border-blue-300/50'"
          >
            <!-- Shine effect on hover -->
            <div class="absolute inset-0 bg-gradient-to-br from-white/0 via-white/5 to-white/0 opacity-0 group-hover:opacity-100 transition-opacity duration-700"></div>
            
            <div class="flex flex-col md:flex-row items-center md:items-start gap-8 relative z-10">
              <!-- Avatar with Premium Glow -->
              <div class="relative cursor-pointer group/avatar flex-shrink-0" @click="triggerAvatarUpload">
                <div class="relative">
                  <!-- Glow effect -->
                  <div class="absolute -inset-2 bg-gradient-to-r from-blue-500 via-purple-500 to-pink-500 rounded-3xl blur-xl opacity-30 group-hover/avatar:opacity-60 transition-opacity duration-300 animate-pulse"></div>
                  
                  <img
                    :src="user.avatar || '/avatar.jpg'"
                    class="relative w-36 h-36 md:w-48 md:h-48 rounded-3xl object-cover shadow-2xl ring-4 transition-all duration-500 group-hover/avatar:scale-105 group-hover/avatar:rotate-2"
                    :class="darkMode ? 'ring-slate-800/50' : 'ring-white/50'"
                    alt="User avatar"
                  />
                  <div
                    class="absolute inset-0 bg-gradient-to-t from-black/90 via-black/50 to-transparent flex items-end justify-center rounded-3xl opacity-0 group-hover/avatar:opacity-100 transition-all duration-300"
                  >
                    <div class="pb-6 flex flex-col items-center gap-2 transform translate-y-4 group-hover/avatar:translate-y-0 transition-transform duration-300">
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-10 w-10 text-white"
                        fill="none"
                        viewBox="0 0 24 24"
                        stroke="currentColor"
                      >
                        <path
                          stroke-linecap="round"
                          stroke-linejoin="round"
                          stroke-width="2"
                          d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z"
                        />
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 13a3 3 0 11-6 0 3 3 0 016 0z" />
                      </svg>
                      <span class="text-sm text-white font-bold">Change Photo</span>
                    </div>
                  </div>
                  
                  <!-- Premium Badge with Animation -->
                  <div
                    v-if="user.is_premium"
                    class="absolute -top-3 -right-3 bg-gradient-to-br from-yellow-400 via-amber-500 to-orange-500 rounded-full p-3 shadow-2xl ring-4 animate-bounce-slow"
                    :class="darkMode ? 'ring-slate-900' : 'ring-white'"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="w-6 h-6 text-white"
                      fill="currentColor"
                      viewBox="0 0 24 24"
                    >
                      <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z" />
                    </svg>
                  </div>
                </div>
                <input
                  ref="avatarInput"
                  type="file"
                  class="hidden"
                  accept="image/*"
                  @change="changeAvatar"
                  :disabled="isUploading"
                />
              </div>

              <!-- User Info -->
              <div class="flex-1 text-center md:text-left">
                <div class="mb-6">
                  <h1 class="text-4xl md:text-5xl font-black mb-3 bg-gradient-to-r from-blue-600 via-purple-600 to-pink-600 bg-clip-text text-transparent animate-gradient">
                    {{ user.name || 'User' }}
                  </h1>
                  <p
                    class="text-base md:text-lg font-medium transition-colors flex items-center justify-center md:justify-start gap-2"
                    :class="darkMode ? 'text-gray-400' : 'text-gray-600'"
                  >
                    <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z" />
                    </svg>
                    {{ user.email || 'N/A' }}
                  </p>
                </div>

                <!-- Premium Button with Effects -->
                <a
                  href="https://t.me/Meteorpremium_bot"
                  target="_blank"
                  class="inline-flex items-center gap-3 px-8 py-4 rounded-2xl font-bold shadow-2xl hover:shadow-3xl transition-all duration-300 hover:scale-105 relative overflow-hidden group/btn"
                  :class="user.is_premium
                    ? 'bg-gradient-to-r from-green-500 via-emerald-500 to-teal-500 text-white'
                    : 'bg-gradient-to-r from-yellow-400 via-amber-500 to-orange-500 text-white'"
                >
                  <!-- Button shine effect -->
                  <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/30 to-transparent transform -skew-x-12 translate-x-full group-hover/btn:translate-x-[-200%] transition-transform duration-1000"></div>
                  
                  <svg
                    v-if="!user.is_premium"
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-6 h-6 animate-pulse"
                    fill="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z" />
                  </svg>
                  <svg
                    v-else
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-6 h-6"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M5 13l4 4L19 7" />
                  </svg>
                  <span class="relative z-10 text-lg">{{ user.is_premium ? 'Premium Active ✨' : 'Unlock Premium 🚀' }}</span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5 transition-transform group-hover/btn:translate-x-1 group-hover/btn:-translate-y-1"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2.5" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                  </svg>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Content Area -->
    <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 py-10 md:py-16 relative z-10">
      <!-- Tags Section with Modern Design -->
      <div class="mb-12">
        <h2
          class="text-3xl font-black mb-8 transition-colors flex items-center gap-3"
          :class="darkMode ? 'text-gray-100' : 'text-gray-900'"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8 text-blue-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 7h.01M7 3h5c.512 0 1.024.195 1.414.586l7 7a2 2 0 010 2.828l-7 7a2 2 0 01-2.828 0l-7-7A1.994 1.994 0 013 12V7a4 4 0 014-4z" />
          </svg>
          Collections
        </h2>
        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 md:gap-5">
          <button
            v-for="tag in tags"
            :key="tag.name"
            @click="selectTag(tag.name)"
            class="flex flex-col items-center justify-center py-6 md:py-7 rounded-3xl border-2 font-bold transition-all duration-300 shadow-lg hover:shadow-2xl group/tag relative overflow-hidden transform hover:-translate-y-1"
            :class="[
              activeTag === tag.name
                ? tag.activeClass
                : darkMode
                ? 'bg-slate-800/70 text-gray-300 border-slate-700/50 hover:bg-slate-700/70 hover:border-slate-600 backdrop-blur-sm'
                : 'bg-white/90 text-gray-700 border-gray-200/50 hover:bg-white hover:border-gray-300 backdrop-blur-sm',
            ]"
          >
            <!-- Background gradient on hover -->
            <div class="absolute inset-0 bg-gradient-to-br from-white/10 via-transparent to-white/5 opacity-0 group-hover/tag:opacity-100 transition-opacity duration-300"></div>
            
            <component :is="tag.icon" class="w-8 h-8 mb-3 transition-all duration-300 group-hover/tag:scale-125 group-hover/tag:rotate-12 relative z-10" />
            <span class="text-base md:text-lg relative z-10">{{ tag.label }}</span>
            
            <!-- Active indicator -->
            <div v-if="activeTag === tag.name" class="absolute bottom-2 w-12 h-1 bg-white/50 rounded-full"></div>
          </button>
        </div>
      </div>

      <!-- Stories Section -->
      <div>
        <div class="flex items-center justify-between mb-8">
          <h2
            class="text-3xl font-black transition-colors flex items-center gap-3"
            :class="darkMode ? 'text-gray-100' : 'text-gray-900'"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8 text-purple-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
            </svg>
            {{ activeTag ? tags.find(t => t.name === activeTag)?.label : 'My Library' }}
          </h2>
          <span
            v-if="!isLoadingStories && stories.length > 0"
            class="text-sm font-bold px-5 py-2 rounded-full transition-colors backdrop-blur-sm"
            :class="darkMode ? 'bg-slate-800/70 text-blue-400' : 'bg-blue-100 text-blue-700'"
          >
            {{ stories.length }} {{ stories.length === 1 ? 'item' : 'items' }}
          </span>
        </div>

        <!-- Loading State with Style -->
        <div v-if="isLoadingStories" class="flex items-center justify-center py-32">
          <div class="flex flex-col items-center gap-6">
            <div class="relative">
              <div
                class="w-16 h-16 border-4 border-t-transparent rounded-full animate-spin"
                :class="darkMode ? 'border-blue-500' : 'border-blue-600'"
              ></div>
              <div
                class="absolute inset-0 w-16 h-16 border-4 border-b-transparent rounded-full animate-spin-reverse"
                :class="darkMode ? 'border-purple-500' : 'border-purple-600'"
                style="animation-delay: 0.3s"
              ></div>
            </div>
            <p class="text-lg font-semibold" :class="darkMode ? 'text-gray-300' : 'text-gray-700'">Loading your collection...</p>
          </div>
        </div>

        <!-- Stories Grid with Hover Effects -->
        <div
          v-else-if="stories.length"
          class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-5 md:gap-6"
        >
          <div
            v-for="story in stories"
            :key="story.id"
            class="group/card cursor-pointer"
            @click="goToAnime(story.id)"
          >
            <div
              class="rounded-3xl overflow-hidden shadow-xl transition-all duration-500 group-hover/card:shadow-3xl group-hover/card:-translate-y-3 relative"
              :class="darkMode ? 'bg-slate-800/80 backdrop-blur-sm' : 'bg-white backdrop-blur-sm'"
            >
              <div class="relative overflow-hidden aspect-[2/3]">
                <img
                  :src="story.poster_url || '/placeholder.jpg'"
                  class="w-full h-full object-cover transition-transform duration-700 group-hover/card:scale-125 group-hover/card:rotate-2"
                  :alt="story.title"
                />
                <!-- Gradient overlay -->
                <div class="absolute inset-0 bg-gradient-to-t from-black via-transparent to-transparent opacity-60 group-hover/card:opacity-80 transition-opacity duration-300"></div>
                
                <!-- Play icon on hover -->
                <div class="absolute inset-0 flex items-center justify-center opacity-0 group-hover/card:opacity-100 transition-all duration-300">
                  <div class="w-16 h-16 bg-white/20 backdrop-blur-md rounded-full flex items-center justify-center transform scale-75 group-hover/card:scale-100 transition-transform duration-300">
                    <svg xmlns="http://www.w3.org/2000/svg" class="w-8 h-8 text-white" fill="currentColor" viewBox="0 0 24 24">
                      <path d="M8 5v14l11-7z"/>
                    </svg>
                  </div>
                </div>
              </div>
              <div class="p-4">
                <h4 class="text-sm font-bold truncate mb-1 transition-colors group-hover/card:text-blue-500">
                  {{ story.title }}
                </h4>
                <p
                  class="text-xs font-semibold uppercase tracking-wide transition-colors"
                  :class="darkMode ? 'text-gray-500' : 'text-gray-500'"
                >
                  {{ story.type || 'Anime' }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Empty State with Character -->
        <div
          v-else
          class="flex flex-col items-center justify-center py-32 rounded-3xl transition-all backdrop-blur-sm"
          :class="darkMode ? 'bg-slate-800/30' : 'bg-white/50'"
        >
          <div
            class="w-32 h-32 rounded-full flex items-center justify-center mb-6 relative"
            :class="darkMode ? 'bg-slate-700/50' : 'bg-gray-200/50'"
          >
            <!-- Animated rings -->
            <div class="absolute inset-0 rounded-full border-4 border-blue-500/20 animate-ping"></div>
            <div class="absolute inset-4 rounded-full border-4 border-purple-500/20 animate-ping" style="animation-delay: 0.5s"></div>
            
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="w-16 h-16 relative z-10"
              :class="darkMode ? 'text-gray-500' : 'text-gray-400'"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
            </svg>
          </div>
          <p
            class="text-2xl font-bold mb-3"
            :class="darkMode ? 'text-gray-200' : 'text-gray-800'"
          >
            No anime yet
          </p>
          <p
            class="text-base"
            :class="darkMode ? 'text-gray-400' : 'text-gray-600'"
          >
            Start building your {{ activeTag || 'library' }} collection
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from "vue";
import { useRouter } from "vue-router";
import {
  EyeIcon,
  PauseCircleIcon,
  CheckCircleIcon,
  HeartIcon,
} from "lucide-vue-next";

const API_BASE = "https://api.meteordub.uz/api";
const getAuthToken = () => localStorage.getItem("access_token");
const router = useRouter();

const darkMode = ref(false);
const user = ref({
  name: "Loading...",
  email: "Loading...",
  avatar: "/avatar.jpg",
  is_premium: false,
});
const background = ref("/hero.jpg");
const avatarInput = ref(null);
const isUploading = ref(false);
const isLoadingStories = ref(false);
const stories = ref([]);
const activeTag = ref(localStorage.getItem("activeTag") || "watching");

// Tags with updated styles
const tags = [
  {
    name: "watching",
    label: "Watching",
    icon: EyeIcon,
    activeClass:
      "bg-gradient-to-br from-blue-500 via-blue-600 to-indigo-600 text-white border-blue-400 shadow-2xl shadow-blue-500/50 scale-105 ring-4 ring-blue-400/30",
  },
  {
    name: "paused",
    label: "Paused",
    icon: PauseCircleIcon,
    activeClass:
      "bg-gradient-to-br from-orange-500 via-red-500 to-pink-600 text-white border-orange-400 shadow-2xl shadow-orange-500/50 scale-105 ring-4 ring-orange-400/30",
  },
  {
    name: "finished",
    label: "Finished",
    icon: CheckCircleIcon,
    activeClass:
      "bg-gradient-to-br from-green-500 via-emerald-600 to-teal-600 text-white border-green-400 shadow-2xl shadow-green-500/50 scale-105 ring-4 ring-green-400/30",
  },
  {
    name: "favorite",
    label: "Favorite",
    icon: HeartIcon,
    activeClass:
      "bg-gradient-to-br from-pink-500 via-rose-600 to-red-600 text-white border-pink-400 shadow-2xl shadow-pink-500/50 scale-105 ring-4 ring-pink-400/30",
  },
];

// =====================
// Fetch User
// =====================
async function fetchMe() {
  const token = getAuthToken();
  if (!token) return;

  try {
    const res = await fetch(`${API_BASE}/me/`, {
      headers: { Authorization: `Bearer ${token}` },
    });
    if (!res.ok) throw new Error("Failed to load user");
    const result = await res.json();
    const data = result.data || {};

    user.value.name = data.full_name || "User";
    user.value.email = data.email || "N/A";
    user.value.avatar = data.avatar_url || "/avatar.jpg";
    user.value.is_premium = data.is_premium || false;
  } catch (err) {
    console.error("Error fetching user:", err);
  }
}

// =====================
// Avatar Upload
// =====================
async function uploadAvatar(file) {
  isUploading.value = true;
  const token = getAuthToken();
  if (!token) {
    alert("Please log in first.");
    isUploading.value = false;
    return;
  }

  const formData = new FormData();
  formData.append("avatar", file);

  try {
    const res = await fetch(`${API_BASE}/me/`, {
      method: "PATCH",
      headers: { Authorization: `Bearer ${token}` },
      body: formData,
    });
    if (!res.ok) throw new Error("Failed to upload avatar");

    await fetchMe();
    alert("Avatar updated successfully!");
  } catch (err) {
    console.error("Avatar upload error:", err);
    alert("Failed to upload avatar.");
  } finally {
    isUploading.value = false;
  }
}

const triggerAvatarUpload = () => {
  if (!isUploading.value) avatarInput.value?.click();
};

const changeAvatar = (e) => {
  const file = e.target.files[0];
  if (file) uploadAvatar(file);
};

// =====================
// Stories by Tag (Local)
// =====================
function getLocalAnimeIds(tag) {
  const key = `${tag}_anime_ids`;
  try {
    const list = localStorage.getItem(key);
    return list ? JSON.parse(list).map((id) => parseInt(id)) : [];
  } catch {
    return [];
  }
}

async function fetchAnimeDetailById(id) {
  try {
    const res = await fetch(`${API_BASE}/animes/${id}/`);
    if (!res.ok) return null;
    const data = (await res.json()).data;
    return {
      id: data.id,
      title: data.title_ru || data.title || "Untitled",
      poster_url: data.poster_url,
      type: data.type,
    };
  } catch {
    return null;
  }
}

async function fetchStoriesByTagFromLocal() {
  stories.value = [];
  if (!activeTag.value) return;
  isLoadingStories.value = true;

  const animeIds = getLocalAnimeIds(activeTag.value);
  const results = await Promise.all(animeIds.map((id) => fetchAnimeDetailById(id)));
  stories.value = results.filter((s) => s);
  isLoadingStories.value = false;
}

// =====================
// Misc
// =====================
const selectTag = (tag) => {
  activeTag.value = activeTag.value === tag ? "" : tag;
  localStorage.setItem("activeTag", activeTag.value);
};
const goBack = () => router.push({ name: "Home" });
const goToAnime = (id) => router.push(`/anime/${id}`);

// =====================
// Lifecycle
// =====================
onMounted(() => {
  darkMode.value = JSON.parse(localStorage.getItem("darkMode")) || false;
  document.documentElement.classList.toggle("dark", darkMode.value);
  fetchMe();
  fetchStoriesByTagFromLocal();
});

watch(activeTag, fetchStoriesByTagFromLocal);
</script>

<style scoped>
@keyframes gradient {
  0%, 100% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
}

.animate-gradient {
  background-size: 200% 200%;
  animation: gradient 8s ease infinite;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0) translateX(0);
  }
  50% {
    transform: translateY(-20px) translateX(10px);
  }
}

.animate-float {
  animation: float 6s ease-in-out infinite;
}

@keyframes spin-slow {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}

.animate-spin-slow {
  animation: spin-slow 20s linear infinite;
}

@keyframes spin-reverse {
  from {
    transform: rotate(360deg);
  }
  to {
    transform: rotate(0deg);
  }
}

.animate-spin-reverse {
  animation: spin-reverse 1s linear infinite;
}

@keyframes bounce-slow {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-5px);
  }
}

.animate-bounce-slow {
  animation: bounce-slow 2s ease-in-out infinite;
}

.shadow-3xl {
  box-shadow: 0 35px 60px -15px rgba(0, 0, 0, 0.3);
}
</style>