<template>
  <div
    class="min-h-screen transition-colors duration-300"
    :class="darkMode ? 'bg-gradient-to-br from-slate-950 via-slate-900 to-slate-950 text-white' : 'bg-gradient-to-br from-gray-50 via-white to-gray-100 text-slate-900'"
  >
    <!-- Header with Banner -->
    <div class="relative w-full">
      <!-- Banner Image -->
      <div
        class="w-full h-64 md:h-80 relative overflow-hidden"
        :style="`background-image: url(${background}); background-size: cover; background-position: center;`"
      >
        <div
          class="absolute inset-0 backdrop-blur-sm transition-colors duration-300"
          :class="darkMode ? 'bg-gradient-to-b from-black/60 via-black/40 to-slate-950' : 'bg-gradient-to-b from-black/30 via-black/20 to-white'"
        ></div>
        
        <!-- Back Button (Top Left) -->
        <button
          @click="goBack"
          class="absolute top-6 left-6 p-3 rounded-2xl backdrop-blur-md transition-all hover:scale-105 shadow-lg z-10"
          :class="darkMode ? 'bg-white/10 hover:bg-white/20 text-white' : 'bg-black/20 hover:bg-black/30 text-white'"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18" />
          </svg>
        </button>
      </div>

      <!-- Profile Card Container -->
      <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="relative -mt-24 md:-mt-32">
          <!-- Profile Card -->
          <div
            class="rounded-3xl shadow-2xl p-6 md:p-8 backdrop-blur-xl transition-all duration-300"
            :class="darkMode 
              ? 'bg-slate-800/80 border border-slate-700/50' 
              : 'bg-white/90 border border-gray-200/50'"
          >
            <div class="flex flex-col md:flex-row items-center md:items-start gap-6">
              <!-- Avatar -->
              <div class="relative cursor-pointer group flex-shrink-0" @click="triggerAvatarUpload">
                <div class="relative">
                  <img
                    :src="user.avatar || '/avatar.jpg'"
                    class="w-32 h-32 md:w-40 md:h-40 rounded-3xl object-cover shadow-xl ring-4 transition-all duration-300 group-hover:scale-105"
                    :class="darkMode ? 'ring-slate-700' : 'ring-white'"
                    alt="User avatar"
                  />
                  <div
                    class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/40 to-transparent flex items-end justify-center rounded-3xl opacity-0 group-hover:opacity-100 transition-opacity duration-300"
                  >
                    <div class="pb-4 flex flex-col items-center gap-1">
                      <svg
                        xmlns="http://www.w3.org/2000/svg"
                        class="h-8 w-8 text-white"
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
                      <span class="text-xs text-white font-medium">Change</span>
                    </div>
                  </div>
                  
                  <!-- Premium Badge -->
                  <div
                    v-if="user.is_premium"
                    class="absolute -top-2 -right-2 bg-gradient-to-br from-amber-400 to-yellow-500 rounded-full p-2 shadow-lg ring-4"
                    :class="darkMode ? 'ring-slate-800' : 'ring-white'"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      class="w-5 h-5 text-yellow-900"
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
                <h1 class="text-3xl md:text-4xl font-bold mb-2">
                  {{ user.name || 'User' }}
                </h1>
                <p
                  class="text-sm md:text-base mb-4 transition-colors"
                  :class="darkMode ? 'text-gray-400' : 'text-gray-600'"
                >
                  {{ user.email || 'N/A' }}
                </p>

                <!-- Premium Button -->
                <a
                  href="https://t.me/shox_p2"
                  target="_blank"
                  class="inline-flex items-center gap-3 px-6 py-3 rounded-2xl font-semibold shadow-lg hover:shadow-xl transition-all duration-300 hover:scale-105"
                  :class="user.is_premium
                    ? darkMode 
                      ? 'bg-gradient-to-r from-green-600 to-emerald-600 text-white'
                      : 'bg-gradient-to-r from-green-500 to-emerald-500 text-white'
                    : darkMode
                      ? 'bg-gradient-to-r from-amber-500 via-yellow-500 to-amber-400 text-gray-900'
                      : 'bg-gradient-to-r from-amber-400 via-yellow-400 to-amber-300 text-gray-900'"
                >
                  <svg
                    v-if="!user.is_premium"
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5"
                    fill="currentColor"
                    viewBox="0 0 24 24"
                  >
                    <path d="M12 2l3.09 6.26L22 9.27l-5 4.87L18.18 22 12 18.56 5.82 22 7 14.14 2 9.27l6.91-1.01L12 2z" />
                  </svg>
                  <svg
                    v-else
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7" />
                  </svg>
                  <span>{{ user.is_premium ? 'Premium Active' : 'Get Premium' }}</span>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-4 h-4"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                  </svg>
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Content Area -->
    <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 py-8 md:py-12">
      <!-- Tags Section -->
      <div class="mb-10">
        <h2
          class="text-2xl font-bold mb-6 transition-colors"
          :class="darkMode ? 'text-gray-100' : 'text-gray-800'"
        >
          Collections
        </h2>
        <div class="grid grid-cols-2 sm:grid-cols-4 gap-3 md:gap-4">
          <button
            v-for="tag in tags"
            :key="tag.name"
            @click="selectTag(tag.name)"
            class="flex flex-col items-center justify-center py-4 md:py-5 rounded-2xl border-2 font-medium transition-all duration-300 shadow-sm hover:shadow-lg group relative overflow-hidden"
            :class="[
              activeTag === tag.name
                ? tag.activeClass
                : darkMode
                ? 'bg-slate-800/50 text-gray-300 border-slate-700/50 hover:bg-slate-700/50 hover:border-slate-600'
                : 'bg-white text-gray-700 border-gray-200 hover:bg-gray-50 hover:border-gray-300',
            ]"
          >
            <div class="absolute inset-0 bg-gradient-to-br from-white/5 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
            <component :is="tag.icon" class="w-6 h-6 mb-2 transition-transform group-hover:scale-110" />
            <span class="text-sm md:text-base relative z-10">{{ tag.label }}</span>
          </button>
        </div>
      </div>

      <!-- Stories Section -->
      <div>
        <div class="flex items-center justify-between mb-6">
          <h2
            class="text-2xl font-bold transition-colors"
            :class="darkMode ? 'text-gray-100' : 'text-gray-800'"
          >
            {{ activeTag ? tags.find(t => t.name === activeTag)?.label : 'My Library' }}
          </h2>
          <span
            v-if="!isLoadingStories && stories.length > 0"
            class="text-sm px-4 py-1.5 rounded-full transition-colors"
            :class="darkMode ? 'bg-slate-800 text-gray-400' : 'bg-gray-200 text-gray-600'"
          >
            {{ stories.length }} {{ stories.length === 1 ? 'item' : 'items' }}
          </span>
        </div>

        <!-- Loading State -->
        <div v-if="isLoadingStories" class="flex items-center justify-center py-20">
          <div class="flex flex-col items-center gap-4">
            <div
              class="w-12 h-12 border-4 border-t-transparent rounded-full animate-spin"
              :class="darkMode ? 'border-blue-500' : 'border-blue-600'"
            ></div>
            <p class="text-sm" :class="darkMode ? 'text-gray-400' : 'text-gray-600'">Loading...</p>
          </div>
        </div>

        <!-- Stories Grid -->
        <div
          v-else-if="stories.length"
          class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4 md:gap-5"
        >
          <div
            v-for="story in stories"
            :key="story.id"
            class="group cursor-pointer"
            @click="goToAnime(story.id)"
          >
            <div
              class="rounded-2xl overflow-hidden shadow-md transition-all duration-300 group-hover:shadow-2xl group-hover:-translate-y-2"
              :class="darkMode ? 'bg-slate-800' : 'bg-white'"
            >
              <div class="relative overflow-hidden aspect-[2/3]">
                <img
                  :src="story.poster_url || '/placeholder.jpg'"
                  class="w-full h-full object-cover transition-transform duration-500 group-hover:scale-110"
                  :alt="story.title"
                />
                <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300"></div>
              </div>
              <div class="p-3">
                <h4 class="text-sm font-semibold truncate mb-1 group-hover:text-blue-500 transition-colors">
                  {{ story.title }}
                </h4>
                <p
                  class="text-xs transition-colors"
                  :class="darkMode ? 'text-gray-400' : 'text-gray-500'"
                >
                  {{ story.type || 'Anime' }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Empty State -->
        <div
          v-else
          class="flex flex-col items-center justify-center py-20 rounded-3xl transition-all"
          :class="darkMode ? 'bg-slate-800/30' : 'bg-gray-100'"
        >
          <div
            class="w-20 h-20 rounded-full flex items-center justify-center mb-4"
            :class="darkMode ? 'bg-slate-700' : 'bg-gray-200'"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="w-10 h-10"
              :class="darkMode ? 'text-gray-500' : 'text-gray-400'"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
            </svg>
          </div>
          <p
            class="text-lg font-medium mb-2"
            :class="darkMode ? 'text-gray-300' : 'text-gray-700'"
          >
            No anime found
          </p>
          <p
            class="text-sm"
            :class="darkMode ? 'text-gray-500' : 'text-gray-500'"
          >
            Start adding anime to your {{ activeTag || 'library' }}
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

// Tags
const tags = [
  {
    name: "watching",
    label: "Watching",
    icon: EyeIcon,
    activeClass:
      "bg-gradient-to-br from-blue-500 to-blue-600 text-white border-blue-500 shadow-lg shadow-blue-500/50 scale-105",
  },
  {
    name: "paused",
    label: "Paused",
    icon: PauseCircleIcon,
    activeClass:
      "bg-gradient-to-br from-orange-500 to-red-500 text-white border-orange-500 shadow-lg shadow-orange-500/50 scale-105",
  },
  {
    name: "finished",
    label: "Finished",
    icon: CheckCircleIcon,
    activeClass:
      "bg-gradient-to-br from-green-500 to-emerald-600 text-white border-green-500 shadow-lg shadow-green-500/50 scale-105",
  },
  {
    name: "favorite",
    label: "Favorite",
    icon: HeartIcon,
    activeClass:
      "bg-gradient-to-br from-pink-500 to-rose-600 text-white border-pink-500 shadow-lg shadow-pink-500/50 scale-105",
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