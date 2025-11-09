<template>
  <header
    class="sticky top-0 z-50 backdrop-blur-xl border-b transition-all duration-300"
    :class="darkMode 
      ? 'bg-slate-900/80 border-slate-800 shadow-lg shadow-slate-900/50' 
      : 'bg-white/80 border-gray-200 shadow-md shadow-gray-200/50'"
  >
    <div class="max-w-7xl mx-auto px-4 py-3">
      <div class="flex items-center justify-between gap-3">
        <!-- Left Section: Logo + Avatar -->
        <div class="flex items-center gap-3 sm:gap-4">
          <!-- Logo -->
          <div 
            @click="goHome" 
            class="cursor-pointer group flex items-center gap-2"
          >
            <div class="relative">
              <img 
                :src="logoSrc" 
                alt="Logo"
                class="w-10 h-10 sm:w-12 sm:h-12 object-contain transition-transform duration-300 group-hover:scale-110 group-hover:rotate-6" 
              />
              <div class="absolute inset-0 bg-gradient-to-r from-purple-500/20 to-pink-500/20 rounded-full blur-lg opacity-0 group-hover:opacity-100 transition-opacity"></div>
            </div>
            <span class="hidden md:block text-xl font-bold bg-gradient-to-r from-purple-600 to-pink-600 bg-clip-text text-transparent">
              MeteorDub
            </span>
          </div>

          <!-- User Avatar -->
          <button
            @click="openProfile"
            class="relative group"
          >
            <div class="relative">
              <img
                :src="user?.avatar || '/avatar.jpg'"
                alt="avatar"
                class="w-9 h-9 sm:w-10 sm:h-10 rounded-full border-2 object-cover transition-all duration-300 group-hover:scale-110"
                :class="darkMode ? 'border-purple-500' : 'border-purple-400'"
              />
              <!-- Online indicator -->
              <div class="absolute bottom-0 right-0 w-3 h-3 bg-green-500 rounded-full border-2 border-white animate-pulse"></div>
            </div>
          </button>
        </div>

        <!-- Center: Search (Desktop) -->
        <div class="hidden lg:flex flex-1 max-w-xl mx-8">
          <div class="relative w-full group">
            <input
              v-model="search"
              @input="emitSearch"
              type="text"
              :placeholder="$t('searchPlaceholder')"
              class="w-full pl-12 pr-12 py-3 rounded-2xl border-2 focus:outline-none transition-all duration-300 text-sm"
              :class="darkMode
                ? 'bg-slate-800/50 border-slate-700 placeholder-slate-400 text-white focus:border-purple-500 focus:bg-slate-800'
                : 'bg-gray-50 border-gray-200 placeholder-gray-500 text-slate-900 focus:border-purple-500 focus:bg-white'"
            />
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-5 w-5 absolute left-4 top-1/2 -translate-y-1/2 transition-colors"
              :class="darkMode ? 'text-slate-400 group-focus-within:text-purple-400' : 'text-gray-500 group-focus-within:text-purple-500'"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
            <button
              v-if="search"
              @click="search = ''; emitSearch()"
              class="absolute right-4 top-1/2 -translate-y-1/2 hover:scale-110 transition-transform"
              :class="darkMode ? 'text-slate-400 hover:text-white' : 'text-gray-500 hover:text-gray-900'"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
        </div>

        <!-- Right Section: Action Buttons -->
        <div class="flex items-center gap-2">
          <!-- Mobile Search Toggle -->
          <button
            @click.stop="toggleSearch"
            class="lg:hidden flex items-center justify-center w-10 h-10 rounded-xl transition-all duration-300"
            :class="darkMode 
              ? 'hover:bg-slate-800 text-white' 
              : 'hover:bg-gray-100 text-slate-700'"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
            </svg>
          </button>

          <!-- Filter Button -->
          <button 
            @click.stop="toggleFilter"
            class="flex items-center gap-2 px-3 sm:px-4 h-10 rounded-xl font-medium transition-all duration-300 group"
            :class="darkMode 
              ? 'bg-slate-800 hover:bg-slate-700 text-white' 
              : 'bg-gray-100 hover:bg-gray-200 text-slate-800'"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 transition-transform group-hover:rotate-12" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4" />
            </svg>
            <span class="hidden sm:inline text-sm">{{ $t('filter') }}</span>
          </button>

          <!-- Genres Button -->
          <button 
            @click.stop="toggleGenres"
            class="flex items-center gap-2 px-3 sm:px-4 h-10 rounded-xl font-medium transition-all duration-300"
            :class="darkMode 
              ? 'bg-slate-800 hover:bg-slate-700 text-white' 
              : 'bg-gray-100 hover:bg-gray-200 text-slate-800'"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 7h.01M7 3h5c.512 0 1.024.195 1.414.586l7 7a2 2 0 010 2.828l-7 7a2 2 0 01-2.828 0l-7-7A1.994 1.994 0 013 12V7a4 4 0 014-4z" />
            </svg>
            <span class="hidden sm:inline text-sm">{{ $t('genres') }}</span>
          </button>

          <!-- Settings / Login -->
          <button 
            v-if="user" 
            @click.stop="toggleSettings"
            class="flex items-center justify-center w-10 h-10 rounded-xl transition-all duration-300 relative group"
            :class="darkMode 
              ? 'hover:bg-slate-800 text-white' 
              : 'hover:bg-gray-100 text-slate-800'"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 transition-transform group-hover:rotate-90" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
            </svg>
          </button>

          <router-link 
            v-else 
            to="/login"
            class="flex items-center gap-2 h-10 px-4 rounded-xl font-semibold transition-all duration-300 bg-gradient-to-r from-purple-500 to-pink-600 hover:from-purple-600 hover:to-pink-700 text-white shadow-lg shadow-purple-500/30 hover:shadow-purple-500/50 hover:scale-105"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M11 16l-4-4m0 0l4-4m-4 4h14m-5 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h7a3 3 0 013 3v1" />
            </svg>
            <span class="text-sm">{{ $t('login') }}</span>
          </router-link>
        </div>
      </div>
    </div>

    <!-- Mobile Search Expandable -->
    <transition name="slide-down">
      <div 
        v-if="showMobileSearch"
        class="lg:hidden border-t px-4 py-3"
        :class="darkMode ? 'bg-slate-900/95 border-slate-800' : 'bg-white/95 border-gray-200'"
      >
        <div class="relative">
          <input
            v-model="search"
            @input="emitSearch"
            type="text"
            :placeholder="$t('searchPlaceholder')"
            class="w-full pl-12 pr-12 py-3 rounded-2xl border-2 focus:outline-none transition-all duration-300"
            :class="darkMode
              ? 'bg-slate-800 border-slate-700 placeholder-slate-400 text-white focus:border-purple-500'
              : 'bg-gray-50 border-gray-200 placeholder-gray-500 text-slate-900 focus:border-purple-500'"
          />
          <svg
            xmlns="http://www.w3.org/2000/svg"
            class="h-5 w-5 absolute left-4 top-1/2 -translate-y-1/2"
            :class="darkMode ? 'text-slate-400' : 'text-gray-500'"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
          >
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
          <button
            v-if="search"
            @click="search = ''; emitSearch()"
            class="absolute right-4 top-1/2 -translate-y-1/2"
            :class="darkMode ? 'text-slate-400' : 'text-gray-500'"
          >
            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </button>
        </div>
      </div>
    </transition>

    <!-- Settings Dropdown -->
    <transition name="fade-scale">
      <div 
        v-if="showSettings"
        class="absolute right-4 top-[calc(100%+8px)] w-72 rounded-2xl shadow-2xl border backdrop-blur-xl z-50 overflow-hidden"
        :class="darkMode 
          ? 'bg-slate-800/95 border-slate-700' 
          : 'bg-white/95 border-gray-200'"
        @click.stop
      >
        <!-- Header -->
        <div class="p-4 border-b" :class="darkMode ? 'border-slate-700' : 'border-gray-200'">
          <div class="flex items-center justify-between">
            <h3 class="font-bold text-lg">{{ $t('settings') }}</h3>
            <button 
              @click="showSettings = false"
              class="w-8 h-8 rounded-lg flex items-center justify-center transition-colors"
              :class="darkMode ? 'hover:bg-slate-700' : 'hover:bg-gray-100'"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
        </div>

        <!-- Settings Content -->
        <div class="p-4 space-y-3">
          <!-- Dark Mode Toggle -->
          <div class="flex items-center justify-between p-3 rounded-xl transition-colors" :class="darkMode ? 'hover:bg-slate-700' : 'hover:bg-gray-50'">
            <div class="flex items-center gap-3">
              <div class="w-10 h-10 rounded-lg flex items-center justify-center" :class="darkMode ? 'bg-slate-700' : 'bg-gray-100'">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
                </svg>
              </div>
              <span class="font-medium">{{ darkMode ? $t('darkTheme') : $t('lightMode') }}</span>
            </div>
            <label class="relative inline-flex items-center cursor-pointer">
              <input type="checkbox" class="sr-only peer" v-model="darkMode" @change="toggleDarkMode" />
              <div class="w-12 h-6 rounded-full transition-all peer-checked:bg-purple-600" :class="darkMode ? 'bg-purple-600' : 'bg-gray-300'"></div>
              <div class="absolute left-1 top-1 bg-white h-4 w-4 rounded-full transition-all peer-checked:translate-x-6 shadow-md"></div>
            </label>
          </div>

          <!-- Language Toggle -->
          <button 
            @click="toggleLang"
            class="w-full flex items-center gap-3 p-3 rounded-xl transition-all font-medium"
            :class="darkMode ? 'hover:bg-slate-700' : 'hover:bg-gray-50'"
          >
            <div class="w-10 h-10 rounded-lg flex items-center justify-center" :class="darkMode ? 'bg-slate-700' : 'bg-gray-100'">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5h12M9 3v2m1.048 9.5A18.022 18.022 0 016.412 9m6.088 9h7M11 21l5-10 5 10M12.751 5C11.783 10.77 8.07 15.61 3 18.129" />
              </svg>
            </div>
            <span>{{ $t('languageText') }}: <span class="font-bold">{{ locale }}</span></span>
          </button>

          <!-- Logout -->
          <button 
            @click="doLogout"
            class="w-full flex items-center gap-3 p-3 rounded-xl transition-all bg-red-500/10 hover:bg-red-500 hover:text-white font-medium text-red-600"
          >
            <div class="w-10 h-10 rounded-lg flex items-center justify-center bg-red-500/20">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" />
              </svg>
            </div>
            <span>{{ $t('logout') }}</span>
          </button>
        </div>
      </div>
    </transition>

    <!-- Genres Dropdown -->
    <transition name="fade-scale">
      <div 
        v-if="showGenres"
        class="absolute right-4 top-[calc(100%+8px)] w-80 max-h-[500px] overflow-y-auto rounded-2xl shadow-2xl border backdrop-blur-xl z-50"
        :class="darkMode ? 'bg-slate-800/95 border-slate-700' : 'bg-white/95 border-gray-200'"
        @click.stop
      >
        <div class="sticky top-0 p-4 border-b backdrop-blur-xl" :class="darkMode ? 'bg-slate-800/95 border-slate-700' : 'bg-white/95 border-gray-200'">
          <div class="flex items-center justify-between mb-3">
            <h3 class="font-bold text-lg">{{ $t('selectGenres') }}</h3>
            <button 
              @click="showGenres = false"
              class="w-8 h-8 rounded-lg flex items-center justify-center transition-colors"
              :class="darkMode ? 'hover:bg-slate-700' : 'hover:bg-gray-100'"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
        </div>
        
        <div class="p-4">
          <div class="grid grid-cols-2 gap-2 mb-4">
            <label 
              v-for="genre in genresList" 
              :key="genre.id"
              class="flex items-center gap-2 p-3 rounded-xl cursor-pointer transition-all"
              :class="[
                selectedGenres.includes(genre.slug || genre.name)
                  ? darkMode
                    ? 'bg-purple-600 text-white'
                    : 'bg-purple-500 text-white'
                  : darkMode
                    ? 'bg-slate-700 hover:bg-slate-600'
                    : 'bg-gray-100 hover:bg-gray-200'
              ]"
            >
              <input 
                type="checkbox" 
                :value="genre.slug || genre.name" 
                v-model="selectedGenres"
                class="hidden"
              />
              <span class="text-sm font-medium truncate">{{ genre.name }}</span>
            </label>
          </div>
          
          <button 
            @click="applyFilterGenres"
            class="w-full py-3 rounded-xl font-bold transition-all bg-gradient-to-r from-purple-500 to-pink-600 hover:from-purple-600 hover:to-pink-700 text-white shadow-lg hover:shadow-xl"
          >
            {{ $t('apply') }} ({{ selectedGenres.length }})
          </button>
        </div>
      </div>
    </transition>

    <!-- Filter Dropdown -->
    <transition name="fade-scale">
      <div 
        v-if="showFilter"
        class="absolute right-4 top-[calc(100%+8px)] w-80 max-h-[600px] overflow-y-auto rounded-2xl shadow-2xl border backdrop-blur-xl z-50"
        :class="darkMode ? 'bg-slate-800/95 border-slate-700' : 'bg-white/95 border-gray-200'"
        @click.stop
      >
        <div class="sticky top-0 p-4 border-b backdrop-blur-xl" :class="darkMode ? 'bg-slate-800/95 border-slate-700' : 'bg-white/95 border-gray-200'">
          <div class="flex items-center justify-between">
            <h3 class="font-bold text-lg">{{ $t('filter') }}</h3>
            <button 
              @click="showFilter = false"
              class="w-8 h-8 rounded-lg flex items-center justify-center transition-colors"
              :class="darkMode ? 'hover:bg-slate-700' : 'hover:bg-gray-100'"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
              </svg>
            </button>
          </div>
        </div>

        <div class="p-4 space-y-4">
          <!-- Type -->
          <div>
            <label class="block font-medium mb-2 text-sm">{{ $t('type') }}</label>
            <select 
              v-model="filter.type"
              class="w-full border-2 rounded-xl px-4 py-2.5 focus:outline-none focus:ring-2 focus:ring-purple-500 transition-all"
              :class="darkMode ? 'bg-slate-700 border-slate-600 text-white' : 'bg-white border-gray-200 text-slate-800'"
            >
              <option value="">{{ $t('any') }}</option>
              <option value="TV">{{ $t('series') }}</option>
              <option value="Movie">{{ $t('movie') }}</option>
            </select>
          </div>

          <!-- Status -->
          <div>
            <label class="block font-medium mb-2 text-sm">{{ $t('status') }}</label>
            <select 
              v-model="filter.status"
              class="w-full border-2 rounded-xl px-4 py-2.5 focus:outline-none focus:ring-2 focus:ring-purple-500 transition-all"
              :class="darkMode ? 'bg-slate-700 border-slate-600 text-white' : 'bg-white border-gray-200 text-slate-800'"
            >
              <option value="">{{ $t('any') }}</option>
              <option value="Ongoing">{{ $t('ongoing') }}</option>
              <option value="Completed">{{ $t('released') }}</option>
            </select>
          </div>

          <!-- Year Range -->
          <div class="grid grid-cols-2 gap-3">
            <div>
              <label class="block font-medium mb-2 text-sm">{{ $t('yearFrom') }}</label>
              <input 
                type="number" 
                v-model="filter.yearFrom" 
                min="1980" 
                max="2025"
                class="w-full border-2 rounded-xl px-4 py-2.5 focus:outline-none focus:ring-2 focus:ring-purple-500 transition-all"
                :class="darkMode ? 'bg-slate-700 border-slate-600 text-white' : 'bg-white border-gray-200 text-slate-800'"
              />
            </div>
            <div>
              <label class="block font-medium mb-2 text-sm">{{ $t('yearTo') }}</label>
              <input 
                type="number" 
                v-model="filter.yearTo" 
                min="1980" 
                max="2025"
                class="w-full border-2 rounded-xl px-4 py-2.5 focus:outline-none focus:ring-2 focus:ring-purple-500 transition-all"
                :class="darkMode ? 'bg-slate-700 border-slate-600 text-white' : 'bg-white border-gray-200 text-slate-800'"
              />
            </div>
          </div>

          <!-- Sort -->
          <div>
            <label class="block font-medium mb-2 text-sm">{{ $t('sort') }}</label>
            <select 
              v-model="filter.sort"
              class="w-full border-2 rounded-xl px-4 py-2.5 focus:outline-none focus:ring-2 focus:ring-purple-500 transition-all"
              :class="darkMode ? 'bg-slate-700 border-slate-600 text-white' : 'bg-white border-gray-200 text-slate-800'"
            >
              <option value="">{{ $t('noSort') }}</option>
              <option value="-rating">{{ $t('sortByRating') }} ↓</option>
              <option value="rating">{{ $t('sortByRating') }} ↑</option>
              <option value="-release_year">{{ $t('sortByYear') }} ↓</option>
              <option value="release_year">{{ $t('sortByYear') }} ↑</option>
            </select>
          </div>

          <button 
            @click="applyMainFilter"
            class="w-full py-3 rounded-xl font-bold transition-all bg-gradient-to-r from-purple-500 to-pink-600 hover:from-purple-600 hover:to-pink-700 text-white shadow-lg hover:shadow-xl"
          >
            {{ $t('apply') }}
          </button>
        </div>
      </div>
    </transition>

    <!-- Backdrop -->
    <transition name="fade">
      <div 
        v-if="showSettings || showGenres || showFilter"
        @click="closeAll"
        class="fixed inset-0 bg-black/50 backdrop-blur-sm z-40"
      ></div>
    </transition>
  </header>
</template>
<script setup>
import { ref, onMounted, onBeforeUnmount, watch, computed, watchEffect } from "vue";
import { useRouter, useRoute } from "vue-router";
import { useI18n } from 'vue-i18n';
import { logout as authLogout, currentUser, fetchMe } from '../utils/auth'; 

const API_BASE = 'https://api.meteordub.uz/api';

const { t, locale } = useI18n({ useScope: 'global' });
const router = useRouter();
const route = useRoute();

const user = ref(currentUser()); 
const search = ref(route.query.search || "");
const showGenres = ref(false);
const showFilter = ref(false);
const showMobileSearch = ref(false);
const showSettings = ref(false);
const darkMode = ref(JSON.parse(localStorage.getItem("darkMode")) || false);
const selectedGenres = ref([]); 
const logoSrc = ref("/Logo.png"); 
const genresList = ref([]);

const filter = ref({
  search: route.query.search || "", 
  type: route.query.type || "",
  status: route.query.status || "",
  yearFrom: route.query.release_year_gte || "",
  yearTo: route.query.release_year_lte || "",
  ratingFrom: route.query.rating_gte || "",
  sort: route.query.ordering || "",
});

async function fetchGenres() {
  try {
    const res = await fetch(`${API_BASE}/genres/`);
    if (!res.ok) throw new Error(`HTTP error: ${res.status}`);
    const data = await res.json();
    genresList.value = data.data || [];
  } catch (error) {
    console.error("Failed to fetch genres:", error);
    genresList.value = [];
  }
}

// ✅ Debounced Search — updates router query instead of emit()
let searchTimeout = null;
function emitSearch() {
  if (searchTimeout) clearTimeout(searchTimeout);
  searchTimeout = setTimeout(() => {
    const query = { ...route.query };
    if (search.value.trim()) query.search = search.value.trim();
    else delete query.search;
    query.page = 1;
    router.push({ name: "Home", query });
  }, 500);
}

// ✅ Apply filters through query params
function getFilterQueryParams() {
  const params = { ...route.query };
  const f = filter.value;

  if (f.search) params.search = f.search;
  else delete params.search;
  if (f.type) params.type = f.type;
  else delete params.type;
  if (f.status) params.status = f.status;
  else delete params.status;
  if (f.yearFrom) params.release_year_gte = f.yearFrom;
  else delete params.release_year_gte;
  if (f.yearTo) params.release_year_lte = f.yearTo;
  else delete params.release_year_lte;
  if (f.ratingFrom) params.rating_gte = f.ratingFrom;
  else delete params.rating_gte;
  if (f.sort) params.ordering = f.sort;
  else delete params.ordering;

  return params;
}


// --- Search keyboard fix ---
const isKeyboardOpen = ref(false)

function onBlur() {
  // Delay reset to avoid flicker when keyboard closes
  setTimeout(() => {
    isKeyboardOpen.value = false
  }, 300)
}

function applyMainFilter() {
  const newQuery = getFilterQueryParams();
  newQuery.page = 1;
  router.push({ name: "Home", query: newQuery });
  showFilter.value = false;
}

// ✅ Apply selected genres as search query (API uses “search”)
function applyFilterGenres() {
  const query = { ...route.query };

  if (selectedGenres.value.length > 0) {
    // ✅ Use 'genres' instead of 'search'
    query.genres = selectedGenres.value.join(",");
  } else {
    delete query.genres;
  }

  query.page = 1;
  router.push({ name: "Home", query });
  showGenres.value = false;
}


// --- UI Toggles ---
function toggleGenres() { closeAll(); showGenres.value = !showGenres.value; }
function toggleFilter() { closeAll(); showFilter.value = !showFilter.value; }
function toggleSearch() { showMobileSearch.value = !showMobileSearch.value; }
function toggleSettings() { closeAll(); showSettings.value = !showSettings.value; }
function closeAll() { showGenres.value = false; showFilter.value = false; showSettings.value = false; showMobileSearch.value = false; }
function goHome() { router.push({ name: "Home" }); }
function openProfile() { closeAll(); router.push("/profile"); }

// --- Auth ---
function doLogout() {
  authLogout(); 
  user.value = null; 
  router.push("/");
  showSettings.value = false;
}

// --- UI Preferences ---
function toggleLang() {
  const newLang = locale.value === "UZ" ? "RU" : "UZ";
  locale.value = newLang; 
  localStorage.setItem("lang", newLang);
}

function toggleDarkMode() {
  document.documentElement.classList.toggle("dark", darkMode.value);
  localStorage.setItem("darkMode", JSON.stringify(darkMode.value));
  if (!sessionStorage.getItem("themeReloaded")) {
    sessionStorage.setItem("themeReloaded", "true");
    window.location.reload();
  }
}

// --- Load user data ---
async function loadUserData() {
  const localUser = currentUser();
  if (localUser) {
    user.value = localUser; 
    const freshUser = await fetchMe();
    if (freshUser) user.value = freshUser;
    else { authLogout(); user.value = null; }
  } else {
    user.value = null;
  }
}

// --- Lifecycle ---
onMounted(() => {
  sessionStorage.removeItem("themeReloaded");
  if (darkMode.value) document.documentElement.classList.add("dark");
  
  // Only close dropdowns if click is OUTSIDE header
  document.addEventListener("click", (e) => {
    const header = document.querySelector("header");
    if (header && !header.contains(e.target)) {
      closeAll();
    }
  });

  loadUserData(); 
  fetchGenres();
});


onBeforeUnmount(() => {
  window.removeEventListener("click", closeAll);
});

// --- Watch router to sync search/filter ---
watch(() => route.query, (q) => {
  search.value = q.search || "";
  filter.value = {
    search: q.search || "",
    type: q.type || "",
    status: q.status || "",
    yearFrom: q.release_year_gte || "",
    yearTo: q.release_year_lte || "",
    ratingFrom: q.rating_gte || "",
    sort: q.ordering || "",
  };
});
watchEffect(() => {
  if (isKeyboardOpen.value) {
    // Lock scrolling but don't force height (prevents white screen)
    document.body.style.overflow = "hidden"
    document.body.style.position = "fixed"
    document.body.style.width = "100%"
  } else {
    document.body.style.overflow = ""
    document.body.style.position = ""
    document.body.style.width = ""
  }
})
</script>

<style  scoped>
.fade-enter-active, .fade-leave-active { transition: opacity 0.2s; }
.fade-enter-from, .fade-leave-to { opacity: 0; }
.slide-enter-active, .slide-leave-active { transition: all 0.2s ease; }
.slide-enter-from, .slide-leave-to { opacity: 0; transform: translateY(-8px); }

html,
body {
  height: 100%;
  overflow-x: hidden;
  overscroll-behavior: contain; /* stops mobile bounce */
}
</style>