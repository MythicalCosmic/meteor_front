<template>
  <footer
    class="mt-8 py-10 transition-colors duration-500"
    :class="darkMode ? 'bg-slate-900 text-gray-300' : 'bg-gray-100 text-gray-700'"
  >
    <div
      class="max-w-6xl mx-auto px-6 grid grid-cols-1 md:grid-cols-3 gap-8 text-center md:text-left text-sm"
    >
      <div>
        <h3
          class="font-bold text-lg mb-3 transition-colors"
          :class="darkMode ? 'text-white' : 'text-slate-900'"
        >
          {{ $t('footer.social') }}
        </h3>
        <div class="flex justify-center md:justify-start space-x-4 mb-4">
          <a href="https://t.me/meteor_dubbin" class="hover:opacity-80 transition">
            <img src="/telegram.svg" alt="Telegram" class="w-6 h-6" />
          </a>
          <a href="https://youtube.com/@meteordubbing?si=Eyi-hkoKC81z1taq" class="hover:opacity-80 transition">
            <img src="/youtube.svg" alt="YouTube" class="w-6 h-6" />
          </a>
          <a href="https://www.instagram.com/meteor_dubbing" class="hover:opacity-80 transition">
            <img src="/instagram.svg" alt="Instagram" class="w-6 h-6" />
          </a>
        </div>
        <p
          class="text-xs transition-colors"
          :class="darkMode ? 'text-gray-400' : 'text-gray-600'"
        >
          {{ $t('footer.disclaimer') }}
        </p>
      </div>

      <div>
        <h3
          class="font-bold text-lg mb-3 underline underline-offset-2 transition-colors"
          :class="darkMode ? 'text-white' : 'text-slate-900'"
        >
          {{ $t('footer.siteInfo').toUpperCase() }}
        </h3>
        <ul class="space-y-1">
          <li>
            <a
              href="#"
              class="hover:underline transition-colors"
              :class="darkMode ? 'text-gray-300 hover:text-white' : 'text-gray-700 hover:text-slate-900'"
            >
              {{ $t('footer.about') }}
            </a>
          </li>
          <li>
            <a
              href="#"
              class="hover:underline transition-colors"
              :class="darkMode ? 'text-gray-300 hover:text-white' : 'text-gray-700 hover:text-slate-900'"
            >
              {{ $t('footer.agreement') }}
            </a>
          </li>
          <li>
            <a
              href="#"
              class="hover:underline transition-colors"
              :class="darkMode ? 'text-gray-300 hover:text-white' : 'text-gray-700 hover:text-slate-900'"
            >
              {{ $t('footer.policy') }}
            </a>
          </li>
          <li>
            <a
              href="#"
              class="hover:underline transition-colors"
              :class="darkMode ? 'text-gray-300 hover:text-white' : 'text-gray-700 hover:text-slate-900'"
            >
              {{ $t('footer.confidentiality') }}
            </a>
          </li>
        </ul>
      </div>

      <div>
        <h3
          class="font-bold text-lg mb-3 underline underline-offset-2 transition-colors"
          :class="darkMode ? 'text-white' : 'text-slate-900'"
        >
          {{ $t('footer.support').toUpperCase() }}
        </h3>
        <ul class="space-y-1">
          <li>
            <a
              href="https://t.me/shox_p2"
              class="hover:underline transition-colors"
              :class="darkMode ? 'text-gray-300 hover:text-white' : 'text-gray-700 hover:text-slate-900'"
            >
              {{ $t('footer.supportChat') }}
            </a>
          </li>
          <li>
            <a
              href="https://t.me/meteor_dubbin"
              class="hover:underline transition-colors"
              :class="darkMode ? 'text-gray-300 hover:text-white' : 'text-gray-700 hover:text-slate-900'"
            >
              Asosiy Kanal
            </a>
          </li>
          <li>
            <a
              href="https://t.me/Cosmosstudiorasmiy"
              class="hover:underline transition-colors"
              :class="darkMode ? 'text-gray-300 hover:text-white' : 'text-gray-700 hover:text-slate-900'"
            >
              Hamkor Kanal
            </a>
          </li>
        </ul>
      </div>
    </div>

    <div
      class="text-center text-xs mt-8 transition-colors"
      :class="darkMode ? 'text-gray-500' : 'text-gray-500'"
    >
      © {{ new Date().getFullYear() }} {{ $t('footer.copyright') }}
    </div>
  </footer>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useI18n } from 'vue-i18n'; 

const { t } = useI18n(); 

const darkMode = ref(JSON.parse(localStorage.getItem("darkMode")) || false);

onMounted(() => {
  darkMode.value = JSON.parse(localStorage.getItem("darkMode")) || false;

  window.addEventListener("storage", (e) => {
    if (e.key === "darkMode") {
      darkMode.value = JSON.parse(e.newValue);
    }
  });
});
</script>