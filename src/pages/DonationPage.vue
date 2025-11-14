<template>
  <div 
    class="min-h-screen transition-colors duration-500 py-12 px-4"
    :class="darkMode ? 'bg-slate-950' : 'bg-slate-50'"
  >
    <!-- Animated Background -->
    <div class="fixed inset-0 overflow-hidden pointer-events-none">
      <div 
        class="absolute top-0 left-1/4 w-96 h-96 rounded-full blur-3xl opacity-20 animate-pulse"
        :class="darkMode ? 'bg-blue-500' : 'bg-blue-300'"
      ></div>
      <div 
        class="absolute bottom-0 right-1/4 w-96 h-96 rounded-full blur-3xl opacity-20 animate-pulse"
        :class="darkMode ? 'bg-purple-500' : 'bg-purple-300'"
        style="animation-delay: 1s"
      ></div>
    </div>

    <div class="max-w-7xl mx-auto relative z-10">
      <!-- Header -->
      <div class="text-center mb-16">
        <div class="inline-flex items-center gap-3 mb-4">
          <div class="w-12 h-12 rounded-xl bg-gradient-to-br from-blue-500 to-purple-600 flex items-center justify-center">
            <svg class="w-7 h-7 text-white" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
            </svg>
          </div>
          <h1 
            class="text-4xl md:text-5xl font-bold tracking-tight"
            :class="darkMode ? 'text-white' : 'text-slate-900'"
          >
            Meteordub Balance
          </h1>
        </div>
        <p 
          class="text-lg max-w-2xl mx-auto"
          :class="darkMode ? 'text-slate-400' : 'text-slate-600'"
        >
          {{ locale === 'UZ' 
            ? 'Donat uchun: @shox_p2' 
            : 'Donat uchun: @shox_p2' 
          }}
        </p>


      </div>

      <!-- Loading State -->
      <div v-if="loading" class="flex justify-center items-center py-20">
        <div class="animate-spin rounded-full h-16 w-16 border-4 border-blue-500 border-t-transparent"></div>
      </div>

      <!-- Top 3 Podium -->
      <div v-else-if="topDonors.length > 0" class="mb-16">
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 max-w-5xl mx-auto items-end">
          <!-- 2nd Place -->
          <div 
            v-if="topDonors[1]"
            class="order-2 md:order-1 transform hover:scale-105 transition-all duration-300"
          >
            <div 
              class="relative rounded-2xl p-6 border transition-all duration-300"
              :class="darkMode 
                ? 'bg-slate-900 border-slate-700 hover:border-slate-600' 
                : 'bg-white border-slate-200 hover:border-slate-300 shadow-lg'"
            >
              <div class="absolute -top-3 left-1/2 transform -translate-x-1/2 w-10 h-10 rounded-full bg-gradient-to-br from-slate-400 to-slate-600 flex items-center justify-center text-white font-bold text-lg shadow-lg">
                2
              </div>

              <div class="flex flex-col items-center text-center pt-4">
                <img 
                  :src="topDonors[1].avatar" 
                  :alt="topDonors[1].username"
                  class="w-20 h-20 rounded-full mb-4 border-4 border-slate-400 object-cover"
                />
                <h3 class="text-lg font-bold mb-1 truncate w-full" :class="darkMode ? 'text-white' : 'text-slate-900'">
                  {{ topDonors[1].username }}
                </h3>
                <div class="flex items-baseline gap-1 mb-2">
                  <span class="text-2xl font-bold bg-gradient-to-r from-slate-500 to-slate-700 bg-clip-text text-transparent">
                    {{ formatAmount(topDonors[1].total_amount) }}
                  </span>
                  <span class="text-sm" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">UZS</span>
                </div>
                <div class="text-sm" :class="darkMode ? 'text-slate-500' : 'text-slate-500'">
                  {{ topDonors[1].donation_count }} {{ locale === 'UZ' ? 'ta' : 'раз' }}
                </div>
              </div>
            </div>
          </div>

          <!-- 1st Place -->
          <div 
            v-if="topDonors[0]"
            class="order-1 md:order-2 transform md:scale-110 hover:scale-115 transition-all duration-300"
          >
            <div 
              class="relative rounded-2xl p-8 border-2 transition-all duration-300"
              :class="darkMode 
                ? 'bg-gradient-to-br from-blue-900/50 to-purple-900/50 border-blue-500/50' 
                : 'bg-gradient-to-br from-blue-50 to-purple-50 border-blue-300 shadow-2xl'"
            >
              <div class="absolute -top-6 left-1/2 transform -translate-x-1/2">
                <div class="text-5xl animate-bounce">👑</div>
              </div>
              <div class="absolute -top-3 left-1/2 transform -translate-x-1/2 w-12 h-12 rounded-full bg-gradient-to-br from-yellow-400 to-yellow-600 flex items-center justify-center text-white font-bold text-xl shadow-xl">
                1
              </div>

              <div class="flex flex-col items-center text-center pt-6">
                <div class="relative mb-5">
                  <div class="absolute inset-0 bg-gradient-to-r from-blue-500 to-purple-600 rounded-full blur-xl opacity-50 animate-pulse"></div>
                  <img 
                    :src="topDonors[0].avatar" 
                    :alt="topDonors[0].username"
                    class="relative w-28 h-28 rounded-full border-4 border-yellow-400 object-cover"
                  />
                </div>
                <h3 class="text-2xl font-bold mb-2 truncate w-full" :class="darkMode ? 'text-white' : 'text-slate-900'">
                  {{ topDonors[0].username }}
                </h3>
                <div class="flex items-baseline gap-1 mb-3">
                  <span class="text-4xl font-bold bg-gradient-to-r from-yellow-400 to-yellow-600 bg-clip-text text-transparent">
                    {{ formatAmount(topDonors[0].total_amount) }}
                  </span>
                  <span class="text-lg font-semibold" :class="darkMode ? 'text-yellow-400' : 'text-yellow-600'">UZS</span>
                </div>
                <div class="text-sm font-medium" :class="darkMode ? 'text-blue-400' : 'text-blue-600'">
                  {{ topDonors[0].donation_count }} {{ locale === 'UZ' ? 'ta' : 'раз' }}
                </div>
              </div>
            </div>
          </div>

          <!-- 3rd Place -->
          <div 
            v-if="topDonors[2]"
            class="order-3 transform hover:scale-105 transition-all duration-300"
          >
            <div 
              class="relative rounded-2xl p-6 border transition-all duration-300"
              :class="darkMode 
                ? 'bg-slate-900 border-slate-700 hover:border-slate-600' 
                : 'bg-white border-slate-200 hover:border-slate-300 shadow-lg'"
            >
              <div class="absolute -top-3 left-1/2 transform -translate-x-1/2 w-10 h-10 rounded-full bg-gradient-to-br from-orange-400 to-orange-600 flex items-center justify-center text-white font-bold text-lg shadow-lg">
                3
              </div>

              <div class="flex flex-col items-center text-center pt-4">
                <img 
                  :src="topDonors[2].avatar" 
                  :alt="topDonors[2].username"
                  class="w-20 h-20 rounded-full mb-4 border-4 border-orange-400 object-cover"
                />
                <h3 class="text-lg font-bold mb-1 truncate w-full" :class="darkMode ? 'text-white' : 'text-slate-900'">
                  {{ topDonors[2].username }}
                </h3>
                <div class="flex items-baseline gap-1 mb-2">
                  <span class="text-2xl font-bold bg-gradient-to-r from-orange-500 to-orange-700 bg-clip-text text-transparent">
                    {{ formatAmount(topDonors[2].total_amount) }}
                  </span>
                  <span class="text-sm" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">UZS</span>
                </div>
                <div class="text-sm" :class="darkMode ? 'text-slate-500' : 'text-slate-500'">
                  {{ topDonors[2].donation_count }} {{ locale === 'UZ' ? 'ta' : 'раз' }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Other Donors -->
      <div v-if="!loading && otherDonors.length > 0" class="max-w-4xl mx-auto">
        <h2 
          class="text-2xl font-bold mb-6"
          :class="darkMode ? 'text-white' : 'text-slate-900'"
        >
          {{ locale === 'UZ' ? 'Boshqa Donorlar' : 'Другие Доноры' }}
        </h2>

        <div class="space-y-3">
          <div 
            v-for="(donor, index) in otherDonors" 
            :key="donor.id"
            class="rounded-xl p-4 border transition-all duration-300 hover:scale-[1.02]"
            :class="darkMode 
              ? 'bg-slate-900 border-slate-700 hover:border-slate-600' 
              : 'bg-white border-slate-200 hover:border-slate-300 shadow'"
          >
            <div class="flex items-center gap-4">
              <div 
                class="flex-shrink-0 w-10 h-10 rounded-lg flex items-center justify-center font-bold"
                :class="darkMode ? 'bg-slate-800 text-slate-400' : 'bg-slate-100 text-slate-600'"
              >
                {{ index + 4 }}
              </div>

              <img 
                :src="donor.avatar" 
                :alt="donor.username"
                class="w-12 h-12 rounded-full border-2 object-cover"
                :class="darkMode ? 'border-slate-700' : 'border-slate-200'"
              />

              <div class="flex-1 min-w-0">
                <h3 class="font-semibold truncate" :class="darkMode ? 'text-white' : 'text-slate-900'">
                  {{ donor.username }}
                </h3>
                <div class="text-sm" :class="darkMode ? 'text-slate-500' : 'text-slate-500'">
                  {{ donor.donation_count }} {{ locale === 'UZ' ? 'ta' : 'раз' }}
                </div>
              </div>

              <div class="text-right">
                <div class="text-xl font-bold" :class="darkMode ? 'text-white' : 'text-slate-900'">
                  {{ formatAmount(donor.total_amount) }}
                </div>
                <div class="text-xs" :class="darkMode ? 'text-slate-500' : 'text-slate-500'">
                  UZS
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Empty State -->
      <div 
        v-if="!loading && donors.length === 0"
        class="text-center py-20"
      >
        <div class="text-6xl mb-4">🎁</div>
        <p class="text-xl" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">
          {{ locale === 'UZ' ? 'Hozircha donorlar yo\'q' : 'Пока нет доноров' }}
        </p>
      </div>

      <!-- Donate Button -->
      <div class="text-center mt-16">
        <a 
          href="https://t.me/shox_p2"
          target="_blank"
          class="inline-flex items-center gap-3 px-8 py-4 rounded-xl font-bold text-lg transition-all duration-300 transform hover:scale-105 shadow-lg hover:shadow-2xl"
          :class="darkMode 
            ? 'bg-gradient-to-r from-blue-600 to-purple-600 text-white hover:from-blue-500 hover:to-purple-500' 
            : 'bg-gradient-to-r from-blue-500 to-purple-500 text-white hover:from-blue-600 hover:to-purple-600'"
        >
          <svg class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
          <span>{{ locale === 'UZ' ? 'Donat qilish' : 'Сделать донат' }}</span>
        </a>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';

const locale = ref(localStorage.getItem('lang') || 'UZ');
const darkMode = ref(true);
const donors = ref([]);
const loading = ref(true);

const API_BASE = 'https://api.meteordub.uz/api';

const topDonors = computed(() => donors.value.slice(0, 3));
const otherDonors = computed(() => donors.value.slice(3));

function formatAmount(amount) {
  if (!amount) return '0';
  return new Intl.NumberFormat('uz-UZ').format(Number(amount));
}

async function fetchDonors() {
  try {
    loading.value = true;

    const response = await fetch(`${API_BASE}/donations/`);
    if (!response.ok) throw new Error('Failed to fetch donations');

    const res = await response.json();

    if (res.success && res.data) {
      // Map top donors (has 'user' object)
      const topDonorsData = (res.data.top_donors || []).map(d => {
        const user = d.user || {};
        return {
          id: user.id ?? d.id ?? Math.random(), // fallback random ID
          username: user.full_name || user.email?.split('@')[0] || 'User',
          email: user.email || '',
          total_amount: d.total_amount || 0,
          donation_count: d.donation_count || 0,
          avatar: user.avatar || `https://api.dicebear.com/7.x/avataaars/svg?seed=${user.id ?? Math.random()}`
        };
      });

      // Map other donors (flat structure)
      const otherDonorsData = (res.data.other_donors || []).map(d => ({
        id: d.id ?? Math.random(),
        username: d.name || d.email?.split('@')[0] || 'User',
        email: d.email || '',
        total_amount: d.amount || 0,
        donation_count: d.donation_count || 1,
        avatar: d.avatar || `https://api.dicebear.com/7.x/avataaars/svg?seed=${d.id ?? Math.random()}`
      }));

      // Combine
      donors.value = [...topDonorsData, ...otherDonorsData];
    } else {
      donors.value = [];
    }
  } catch (error) {
    console.error('Error fetching donors:', error);
    donors.value = [];
  } finally {
    loading.value = false;
  }
}


onMounted(() => {
  fetchDonors();
});
</script>

<style scoped>
.hover\:scale-115:hover {
  transform: scale(1.15);
}
</style>