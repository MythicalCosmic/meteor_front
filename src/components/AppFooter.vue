<template>
  <footer
    class="relative mt-16 overflow-hidden transition-colors duration-500"
    :class="darkMode ? 'bg-gradient-to-b from-slate-950 via-slate-900 to-slate-950' : 'bg-gradient-to-b from-gray-50 via-white to-gray-100'"
  >
    <!-- Animated Background Pattern -->
    <div class="absolute inset-0 opacity-5">
      <div class="absolute inset-0" style="background-image: radial-gradient(circle at 2px 2px, currentColor 1px, transparent 0); background-size: 40px 40px;"></div>
    </div>

    <!-- Decorative Gradient Line -->
    <div class="absolute top-0 left-0 right-0 h-1 bg-gradient-to-r from-purple-500 via-pink-500 to-purple-500"></div>

    <!-- Main Footer Content -->
    <div class="relative max-w-7xl mx-auto px-6 py-16">
      
      <!-- Top Section - Brand & Social -->
      <div class="text-center mb-12">
        <!-- Logo/Brand -->
        <div class="inline-flex items-center justify-center mb-6">
          <div class="relative">
            <div class="absolute inset-0 bg-gradient-to-r from-purple-500 to-pink-500 rounded-2xl blur-xl opacity-50 animate-pulse"></div>
            <div 
              class="relative px-8 py-4 rounded-2xl font-black text-3xl bg-gradient-to-r from-purple-600 to-pink-600 text-transparent bg-clip-text"
            >
              METEOR<span class="text-white">DUB</span>
            </div>
          </div>
        </div>

        <!-- Social Media Icons -->
        <div class="flex justify-center items-center gap-4 mb-6">
          <a 
            href="https://t.me/meteor_dubbin" 
            target="_blank"
            class="group relative w-14 h-14 rounded-2xl flex items-center justify-center transition-all duration-300 hover:scale-110 hover:-translate-y-1"
            :class="darkMode ? 'bg-slate-800 hover:bg-gradient-to-br hover:from-blue-500 hover:to-blue-600' : 'bg-white hover:bg-gradient-to-br hover:from-blue-500 hover:to-blue-600 shadow-md hover:shadow-blue-500/50'"
          >
            <div class="absolute inset-0 rounded-2xl bg-gradient-to-br from-blue-500 to-blue-600 opacity-0 group-hover:opacity-100 transition-opacity"></div>
            <svg class="relative w-6 h-6 transition-colors" :class="darkMode ? 'text-gray-300 group-hover:text-white' : 'text-gray-600 group-hover:text-white'" fill="currentColor" viewBox="0 0 24 24">
              <path d="M12 0C5.373 0 0 5.373 0 12s5.373 12 12 12 12-5.373 12-12S18.627 0 12 0zm5.894 8.221l-1.97 9.28c-.145.658-.537.818-1.084.508l-3-2.21-1.446 1.394c-.14.18-.357.295-.6.295-.002 0-.003 0-.005 0l.213-3.054 5.56-5.022c.24-.213-.054-.334-.373-.121l-6.869 4.326-2.96-.924c-.64-.203-.658-.64.135-.954l11.566-4.458c.538-.196 1.006.128.832.941z"/>
            </svg>
          </a>

          <a 
            href="https://youtube.com/@meteordubbing?si=Eyi-hkoKC81z1taq" 
            target="_blank"
            class="group relative w-14 h-14 rounded-2xl flex items-center justify-center transition-all duration-300 hover:scale-110 hover:-translate-y-1"
            :class="darkMode ? 'bg-slate-800 hover:bg-gradient-to-br hover:from-red-500 hover:to-red-600' : 'bg-white hover:bg-gradient-to-br hover:from-red-500 hover:to-red-600 shadow-md hover:shadow-red-500/50'"
          >
            <div class="absolute inset-0 rounded-2xl bg-gradient-to-br from-red-500 to-red-600 opacity-0 group-hover:opacity-100 transition-opacity"></div>
            <svg class="relative w-6 h-6 transition-colors" :class="darkMode ? 'text-gray-300 group-hover:text-white' : 'text-gray-600 group-hover:text-white'" fill="currentColor" viewBox="0 0 24 24">
              <path d="M23.498 6.186a3.016 3.016 0 0 0-2.122-2.136C19.505 3.545 12 3.545 12 3.545s-7.505 0-9.377.505A3.017 3.017 0 0 0 .502 6.186C0 8.07 0 12 0 12s0 3.93.502 5.814a3.016 3.016 0 0 0 2.122 2.136c1.871.505 9.376.505 9.376.505s7.505 0 9.377-.505a3.015 3.015 0 0 0 2.122-2.136C24 15.93 24 12 24 12s0-3.93-.502-5.814zM9.545 15.568V8.432L15.818 12l-6.273 3.568z"/>
            </svg>
          </a>

          <a 
            href="https://www.instagram.com/meteor_dubbing" 
            target="_blank"
            class="group relative w-14 h-14 rounded-2xl flex items-center justify-center transition-all duration-300 hover:scale-110 hover:-translate-y-1"
            :class="darkMode ? 'bg-slate-800 hover:bg-gradient-to-br hover:from-pink-500 hover:to-purple-600' : 'bg-white hover:bg-gradient-to-br hover:from-pink-500 hover:to-purple-600 shadow-md hover:shadow-pink-500/50'"
          >
            <div class="absolute inset-0 rounded-2xl bg-gradient-to-br from-pink-500 to-purple-600 opacity-0 group-hover:opacity-100 transition-opacity"></div>
            <svg class="relative w-6 h-6 transition-colors" :class="darkMode ? 'text-gray-300 group-hover:text-white' : 'text-gray-600 group-hover:text-white'" fill="currentColor" viewBox="0 0 24 24">
              <path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zm0-2.163c-3.259 0-3.667.014-4.947.072-4.358.2-6.78 2.618-6.98 6.98-.059 1.281-.073 1.689-.073 4.948 0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98 1.281.058 1.689.072 4.948.072 3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98-1.281-.059-1.69-.073-4.949-.073zm0 5.838c-3.403 0-6.162 2.759-6.162 6.162s2.759 6.163 6.162 6.163 6.162-2.759 6.162-6.163c0-3.403-2.759-6.162-6.162-6.162zm0 10.162c-2.209 0-4-1.79-4-4 0-2.209 1.791-4 4-4s4 1.791 4 4c0 2.21-1.791 4-4 4zm6.406-11.845c-.796 0-1.441.645-1.441 1.44s.645 1.44 1.441 1.44c.795 0 1.439-.645 1.439-1.44s-.644-1.44-1.439-1.44z"/>
            </svg>
          </a>
        </div>

        <!-- Tagline -->
        <p 
          class="text-sm max-w-2xl mx-auto leading-relaxed"
          :class="darkMode ? 'text-gray-400' : 'text-gray-600'"
        >
          {{ $t('footer.disclaimer') }}
        </p>
      </div>

      <!-- Links Grid -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 mb-12">
        
        <!-- Site Info Column -->
        <div class="text-center md:text-left">
          <h3 
            class="text-lg font-bold mb-4 bg-gradient-to-r from-purple-500 to-pink-500 text-transparent bg-clip-text"
          >
            {{ $t('footer.siteInfo').toUpperCase() }}
          </h3>
          <ul class="space-y-3">
            <li>
              <a
                href="#"
                class="group inline-flex items-center gap-2 transition-all duration-300 hover:translate-x-2"
                :class="darkMode ? 'text-gray-400 hover:text-white' : 'text-gray-600 hover:text-gray-900'"
              >
                <span class="w-1.5 h-1.5 rounded-full bg-purple-500 opacity-0 group-hover:opacity-100 transition-opacity"></span>
                {{ $t('footer.about') }}
              </a>
            </li>
            <li>
              <a
                href="#"
                class="group inline-flex items-center gap-2 transition-all duration-300 hover:translate-x-2"
                :class="darkMode ? 'text-gray-400 hover:text-white' : 'text-gray-600 hover:text-gray-900'"
              >
                <span class="w-1.5 h-1.5 rounded-full bg-purple-500 opacity-0 group-hover:opacity-100 transition-opacity"></span>
                {{ $t('footer.agreement') }}
              </a>
            </li>
            <li>
              <a
                href="#"
                class="group inline-flex items-center gap-2 transition-all duration-300 hover:translate-x-2"
                :class="darkMode ? 'text-gray-400 hover:text-white' : 'text-gray-600 hover:text-gray-900'"
              >
                <span class="w-1.5 h-1.5 rounded-full bg-purple-500 opacity-0 group-hover:opacity-100 transition-opacity"></span>
                {{ $t('footer.policy') }}
              </a>
            </li>
            <li>
              <a
                href="#"
                class="group inline-flex items-center gap-2 transition-all duration-300 hover:translate-x-2"
                :class="darkMode ? 'text-gray-400 hover:text-white' : 'text-gray-600 hover:text-gray-900'"
              >
                <span class="w-1.5 h-1.5 rounded-full bg-purple-500 opacity-0 group-hover:opacity-100 transition-opacity"></span>
                {{ $t('footer.confidentiality') }}
              </a>
            </li>
          </ul>
        </div>

        <!-- Support Column -->
        <div class="text-center md:text-left">
          <h3 
            class="text-lg font-bold mb-4 bg-gradient-to-r from-blue-500 to-cyan-500 text-transparent bg-clip-text"
          >
            {{ $t('footer.support').toUpperCase() }}
          </h3>
          <ul class="space-y-3">
            <li>
              <a
                href="https://t.me/shox_p2"
                target="_blank"
                class="group inline-flex items-center gap-2 transition-all duration-300 hover:translate-x-2"
                :class="darkMode ? 'text-gray-400 hover:text-white' : 'text-gray-600 hover:text-gray-900'"
              >
                <span class="w-1.5 h-1.5 rounded-full bg-blue-500 opacity-0 group-hover:opacity-100 transition-opacity"></span>
                {{ $t('footer.supportChat') }}
              </a>
            </li>
            <li>
              <a
                href="https://t.me/meteor_dubbin"
                target="_blank"
                class="group inline-flex items-center gap-2 transition-all duration-300 hover:translate-x-2"
                :class="darkMode ? 'text-gray-400 hover:text-white' : 'text-gray-600 hover:text-gray-900'"
              >
                <span class="w-1.5 h-1.5 rounded-full bg-blue-500 opacity-0 group-hover:opacity-100 transition-opacity"></span>
                Asosiy Kanal
              </a>
            </li>
            <li>
              <a
                href="https://t.me/Cosmosstudiorasmiy"
                target="_blank"
                class="group inline-flex items-center gap-2 transition-all duration-300 hover:translate-x-2"
                :class="darkMode ? 'text-gray-400 hover:text-white' : 'text-gray-600 hover:text-gray-900'"
              >
                <span class="w-1.5 h-1.5 rounded-full bg-blue-500 opacity-0 group-hover:opacity-100 transition-opacity"></span>
                Hamkor Kanal
              </a>
            </li>
          </ul>
        </div>

        <!-- Newsletter Column -->
        <div class="text-center md:text-left">
          <h3 
            class="text-lg font-bold mb-4 bg-gradient-to-r from-green-500 to-emerald-500 text-transparent bg-clip-text"
          >
            STAY UPDATED
          </h3>
          <p 
            class="text-sm mb-4"
            :class="darkMode ? 'text-gray-400' : 'text-gray-600'"
          >
            Subscribe to get the latest anime releases
          </p>
          <div class="flex gap-2">
            <input
              type="email"
              placeholder="Enter your email"
              class="flex-1 px-4 py-2.5 rounded-xl border transition-all focus:outline-none focus:ring-2 focus:ring-purple-500"
              :class="darkMode ? 'bg-slate-800 border-slate-700 text-white placeholder-gray-500' : 'bg-white border-gray-300 text-gray-900 placeholder-gray-400'"
            />
            <button 
              class="px-6 py-2.5 rounded-xl bg-gradient-to-r from-purple-500 to-pink-500 text-white font-semibold shadow-lg shadow-purple-500/30 hover:shadow-purple-500/50 transition-all duration-300 hover:scale-105"
            >
              <svg class="w-5 h-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6" />
              </svg>
            </button>
          </div>
        </div>

      </div>

      <!-- Bottom Bar -->
      <div 
        class="pt-8 border-t flex flex-col md:flex-row items-center justify-between gap-4"
        :class="darkMode ? 'border-slate-800' : 'border-gray-200'"
      >
        <p 
          class="text-sm"
          :class="darkMode ? 'text-gray-500' : 'text-gray-500'"
        >
          © {{ new Date().getFullYear() }} {{ $t('footer.copyright') }}
        </p>
        
        <div class="flex items-center gap-6 text-sm">
          <a 
            href="#" 
            class="transition-all duration-300 hover:translate-y-[-2px]"
            :class="darkMode ? 'text-gray-500 hover:text-white' : 'text-gray-500 hover:text-gray-900'"
          >
            Privacy Policy
          </a>
          <a 
            href="#" 
            class="transition-all duration-300 hover:translate-y-[-2px]"
            :class="darkMode ? 'text-gray-500 hover:text-white' : 'text-gray-500 hover:text-gray-900'"
          >
            Terms of Service
          </a>
          <a 
            href="#" 
            class="transition-all duration-300 hover:translate-y-[-2px]"
            :class="darkMode ? 'text-gray-500 hover:text-white' : 'text-gray-500 hover:text-gray-900'"
          >
            Contact Us
          </a>
        </div>
      </div>
    </div>

    <!-- Scroll to Top Button -->
    <button
      @click="scrollToTop"
      class="fixed bottom-8 right-8 w-12 h-12 rounded-full bg-gradient-to-r from-purple-500 to-pink-500 text-white shadow-lg shadow-purple-500/50 hover:shadow-purple-500/70 transition-all duration-300 hover:scale-110 flex items-center justify-center z-50"
    >
      <svg class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 10l7-7m0 0l7 7m-7-7v18" />
      </svg>
    </button>
  </footer>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useI18n } from 'vue-i18n'; 

const { t } = useI18n(); 

const darkMode = ref(JSON.parse(localStorage.getItem("darkMode")) || false);

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: 'smooth'
  });
};

onMounted(() => {
  darkMode.value = JSON.parse(localStorage.getItem("darkMode")) || false;

  window.addEventListener("storage", (e) => {
    if (e.key === "darkMode") {
      darkMode.value = JSON.parse(e.newValue);
    }
  });
});
</script>

<style scoped>
@keyframes pulse {
  0%, 100% {
    opacity: 0.5;
  }
  50% {
    opacity: 0.8;
  }
}

.animate-pulse {
  animation: pulse 3s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}
</style>