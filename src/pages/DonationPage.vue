<template>
  <div 
    class="min-h-screen transition-colors duration-300 py-8 px-4"
    :class="darkMode ? 'bg-gradient-to-br from-slate-900 via-purple-900 to-slate-900' : 'bg-gradient-to-br from-purple-50 via-pink-50 to-orange-50'"
  >
    <div class="max-w-7xl mx-auto">
      <!-- Header Section -->
      <div class="text-center mb-12 animate-fade-in">
        <div class="inline-block mb-4">
          <div class="relative">
            <h1 
              class="text-4xl sm:text-5xl md:text-6xl font-black mb-4 bg-gradient-to-r from-amber-400 via-orange-500 to-pink-500 bg-clip-text text-transparent animate-gradient"
            >
              {{ locale === 'UZ' ? '💎 Meteordub Balance' : '💎 Meteordub Balance' }}
            </h1>
            <div class="absolute -inset-1 bg-gradient-to-r from-amber-400 to-pink-500 blur-2xl opacity-30 animate-pulse"></div>
          </div>
        </div>
        <p 
          class="text-lg sm:text-xl max-w-2xl mx-auto"
          :class="darkMode ? 'text-slate-300' : 'text-slate-700'"
        >
          {{ locale === 'UZ' 
            ? '🙏 Bizning eng saxovatli qo\'llab-quvvatlovchilarimiz!' 
            : '🙏 Наши самые щедрые спонсоры!' 
          }}
        </p>
      </div>

      <!-- Top 3 Podium -->
      <div class="mb-16 relative">
        <!-- Decorative elements -->
        <div class="absolute inset-0 flex items-center justify-center opacity-10">
          <div class="w-96 h-96 bg-gradient-to-r from-amber-500 to-pink-500 rounded-full blur-3xl animate-pulse"></div>
        </div>

        <div class="relative grid grid-cols-1 md:grid-cols-3 gap-6 md:gap-4 max-w-5xl mx-auto items-end">
          <!-- 2nd Place -->
          <div 
            class="order-2 md:order-1 transform hover:scale-105 transition-all duration-500 animate-slide-up"
            style="animation-delay: 0.2s"
          >
            <div 
              class="relative rounded-3xl p-6 backdrop-blur-xl border-2 shadow-2xl overflow-hidden group"
              :class="darkMode 
                ? 'bg-slate-800/90 border-slate-600 hover:border-slate-400' 
                : 'bg-white/90 border-gray-200 hover:border-gray-300'"
            >
              <!-- Silver Badge -->
              <div class="absolute top-4 right-4 flex items-center gap-2">
                <div class="w-12 h-12 rounded-full bg-gradient-to-br from-gray-300 to-gray-500 flex items-center justify-center shadow-lg animate-bounce">
                  <span class="text-2xl font-black text-white">2</span>
                </div>
              </div>

              <!-- Avatar -->
              <div class="flex justify-center mb-4">
                <div class="relative">
                  <div class="w-24 h-24 rounded-full overflow-hidden border-4 border-gray-400 shadow-xl transform group-hover:rotate-12 transition-transform duration-500">
                    <img 
                      :src="topDonors[1]?.avatar || '/avatar.jpg'" 
                      :alt="topDonors[1]?.username"
                      class="w-full h-full object-cover"
                    />
                  </div>
                  <div class="absolute -bottom-2 -right-2 bg-gradient-to-r from-gray-300 to-gray-500 rounded-full p-2 shadow-lg">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-white" fill="currentColor" viewBox="0 0 24 24">
                      <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"/>
                    </svg>
                  </div>
                </div>
              </div>

              <!-- Info -->
              <div class="text-center">
                <h3 class="text-xl font-bold mb-2 truncate" :class="darkMode ? 'text-white' : 'text-slate-900'">
                  {{ topDonors[1]?.username || 'User' }}
                </h3>
                <div class="flex items-center justify-center gap-2 mb-3">
                  <span class="text-3xl font-black bg-gradient-to-r from-gray-400 to-gray-600 bg-clip-text text-transparent">
                    {{ formatAmount(topDonors[1]?.total_amount || 0) }}
                  </span>
                  <span class="text-lg" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">UZS</span>
                </div>
                <div class="flex items-center justify-center gap-2 text-sm" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                  <span>{{ topDonors[1]?.donation_count || 0 }} {{ locale === 'UZ' ? 'ta' : 'раз' }}</span>
                </div>
              </div>

              <!-- Shine effect -->
              <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/10 to-transparent -translate-x-full group-hover:translate-x-full transition-transform duration-1000"></div>
            </div>
          </div>

          <!-- 1st Place (Champion) -->
          <div 
            class="order-1 md:order-2 transform md:scale-110 hover:scale-115 transition-all duration-500 animate-slide-up z-10"
          >
            <div 
              class="relative rounded-3xl p-8 backdrop-blur-xl border-4 shadow-2xl overflow-hidden group"
              :class="darkMode 
                ? 'bg-gradient-to-br from-amber-900/90 to-orange-900/90 border-amber-500' 
                : 'bg-gradient-to-br from-amber-100/90 to-orange-100/90 border-amber-400'"
            >
              <!-- Crown -->
              <div class="absolute -top-8 left-1/2 transform -translate-x-1/2">
                <div class="text-6xl animate-bounce">👑</div>
              </div>

              <!-- Gold Badge -->
              <div class="absolute top-4 right-4 flex items-center gap-2">
                <div class="w-16 h-16 rounded-full bg-gradient-to-br from-amber-300 to-amber-600 flex items-center justify-center shadow-2xl animate-pulse">
                  <span class="text-3xl font-black text-white">1</span>
                </div>
              </div>

              <!-- Avatar -->
              <div class="flex justify-center mb-6 mt-4">
                <div class="relative">
                  <div class="absolute inset-0 bg-gradient-to-r from-amber-400 to-orange-500 rounded-full blur-xl animate-pulse"></div>
                  <div class="relative w-32 h-32 rounded-full overflow-hidden border-4 border-amber-400 shadow-2xl transform group-hover:rotate-12 transition-transform duration-500">
                    <img 
                      :src="topDonors[0]?.avatar || '/avatar.jpg'" 
                      :alt="topDonors[0]?.username"
                      class="w-full h-full object-cover"
                    />
                  </div>
                  <div class="absolute -bottom-2 -right-2 bg-gradient-to-r from-amber-400 to-orange-500 rounded-full p-3 shadow-xl animate-bounce">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-white" fill="currentColor" viewBox="0 0 24 24">
                      <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"/>
                    </svg>
                  </div>
                </div>
              </div>

              <!-- Info -->
              <div class="text-center">
                <h3 class="text-2xl font-black mb-3 truncate" :class="darkMode ? 'text-amber-100' : 'text-amber-900'">
                  {{ topDonors[0]?.username || 'Champion' }}
                </h3>
                <div class="flex items-center justify-center gap-2 mb-4">
                  <span class="text-4xl font-black bg-gradient-to-r from-amber-400 to-orange-500 bg-clip-text text-transparent">
                    {{ formatAmount(topDonors[0]?.total_amount || 0) }}
                  </span>
                  <span class="text-xl font-bold" :class="darkMode ? 'text-amber-300' : 'text-amber-700'">UZS</span>
                </div>
                <div class="flex items-center justify-center gap-2 text-sm font-semibold" :class="darkMode ? 'text-amber-300' : 'text-amber-700'">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                  <span>{{ topDonors[0]?.donation_count || 0 }} {{ locale === 'UZ' ? 'ta' : 'раз' }}</span>
                </div>
              </div>

              <!-- Particles -->
              <div class="absolute inset-0 pointer-events-none">
                <div class="absolute top-10 left-10 w-2 h-2 bg-amber-400 rounded-full animate-ping"></div>
                <div class="absolute top-20 right-10 w-2 h-2 bg-orange-400 rounded-full animate-ping" style="animation-delay: 0.5s"></div>
                <div class="absolute bottom-10 left-1/4 w-2 h-2 bg-amber-400 rounded-full animate-ping" style="animation-delay: 1s"></div>
              </div>
            </div>
          </div>

          <!-- 3rd Place -->
          <div 
            class="order-3 transform hover:scale-105 transition-all duration-500 animate-slide-up"
            style="animation-delay: 0.4s"
          >
            <div 
              class="relative rounded-3xl p-6 backdrop-blur-xl border-2 shadow-2xl overflow-hidden group"
              :class="darkMode 
                ? 'bg-slate-800/90 border-slate-600 hover:border-slate-400' 
                : 'bg-white/90 border-gray-200 hover:border-gray-300'"
            >
              <!-- Bronze Badge -->
              <div class="absolute top-4 right-4 flex items-center gap-2">
                <div class="w-12 h-12 rounded-full bg-gradient-to-br from-orange-300 to-orange-600 flex items-center justify-center shadow-lg animate-bounce" style="animation-delay: 0.2s">
                  <span class="text-2xl font-black text-white">3</span>
                </div>
              </div>

              <!-- Avatar -->
              <div class="flex justify-center mb-4">
                <div class="relative">
                  <div class="w-24 h-24 rounded-full overflow-hidden border-4 border-orange-400 shadow-xl transform group-hover:rotate-12 transition-transform duration-500">
                    <img 
                      :src="topDonors[2]?.avatar || '/avatar.jpg'" 
                      :alt="topDonors[2]?.username"
                      class="w-full h-full object-cover"
                    />
                  </div>
                  <div class="absolute -bottom-2 -right-2 bg-gradient-to-r from-orange-300 to-orange-600 rounded-full p-2 shadow-lg">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-white" fill="currentColor" viewBox="0 0 24 24">
                      <path d="M12 2L15.09 8.26L22 9.27L17 14.14L18.18 21.02L12 17.77L5.82 21.02L7 14.14L2 9.27L8.91 8.26L12 2Z"/>
                    </svg>
                  </div>
                </div>
              </div>

              <!-- Info -->
              <div class="text-center">
                <h3 class="text-xl font-bold mb-2 truncate" :class="darkMode ? 'text-white' : 'text-slate-900'">
                  {{ topDonors[2]?.username || 'User' }}
                </h3>
                <div class="flex items-center justify-center gap-2 mb-3">
                  <span class="text-3xl font-black bg-gradient-to-r from-orange-400 to-orange-700 bg-clip-text text-transparent">
                    {{ formatAmount(topDonors[2]?.total_amount || 0) }}
                  </span>
                  <span class="text-lg" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">UZS</span>
                </div>
                <div class="flex items-center justify-center gap-2 text-sm" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                  <span>{{ topDonors[2]?.donation_count || 0 }} {{ locale === 'UZ' ? 'ta' : 'раз' }}</span>
                </div>
              </div>

              <!-- Shine effect -->
              <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/10 to-transparent -translate-x-full group-hover:translate-x-full transition-transform duration-1000"></div>
            </div>
          </div>
        </div>
      </div>

      <!-- Other Donors List -->
      <div class="max-w-4xl mx-auto">
        <h2 
          class="text-2xl sm:text-3xl font-bold mb-6 text-center"
          :class="darkMode ? 'text-white' : 'text-slate-900'"
        >
          {{ locale === 'UZ' ? '🌟 Boshqa Donorlar' : '🌟 Другие Доноры' }}
        </h2>

        <div class="space-y-4">
          <div 
            v-for="(donor, index) in otherDonors" 
            :key="donor.id"
            class="group rounded-2xl p-5 backdrop-blur-xl border-2 shadow-lg hover:shadow-2xl transition-all duration-300 transform hover:-translate-y-1 animate-fade-in"
            :class="darkMode 
              ? 'bg-slate-800/80 border-slate-700 hover:border-slate-500' 
              : 'bg-white/80 border-gray-200 hover:border-gray-300'"
            :style="`animation-delay: ${(index + 3) * 0.1}s`"
          >
            <div class="flex items-center gap-4">
              <!-- Rank -->
              <div 
                class="flex-shrink-0 w-12 h-12 rounded-xl flex items-center justify-center font-black text-xl shadow-lg"
                :class="darkMode 
                  ? 'bg-slate-700 text-slate-300' 
                  : 'bg-gray-100 text-gray-700'"
              >
                {{ index + 4 }}
              </div>

              <!-- Avatar -->
              <div class="flex-shrink-0">
                <div class="w-16 h-16 rounded-full overflow-hidden border-3 shadow-md group-hover:scale-110 transition-transform duration-300"
                  :class="darkMode ? 'border-purple-500' : 'border-purple-400'">
                  <img 
                    :src="donor.avatar || '/avatar.jpg'" 
                    :alt="donor.username"
                    class="w-full h-full object-cover"
                  />
                </div>
              </div>

              <!-- Info -->
              <div class="flex-1 min-w-0">
                <h3 class="text-lg font-bold truncate mb-1" :class="darkMode ? 'text-white' : 'text-slate-900'">
                  {{ donor.username }}
                </h3>
                <div class="flex items-center gap-2 text-sm" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">
                  <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                  </svg>
                  <span>{{ donor.donation_count }} {{ locale === 'UZ' ? 'ta' : 'раз' }}</span>
                </div>
              </div>

              <!-- Amount -->
              <div class="text-right">
                <div class="text-2xl font-black bg-gradient-to-r from-purple-500 to-pink-500 bg-clip-text text-transparent">
                  {{ formatAmount(donor.total_amount) }}
                </div>
                <div class="text-sm font-medium" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">
                  UZS
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Empty State -->
        <div 
          v-if="otherDonors.length === 0"
          class="text-center py-12"
        >
          <div class="text-6xl mb-4">🎁</div>
          <p class="text-xl" :class="darkMode ? 'text-slate-400' : 'text-slate-600'">
            {{ locale === 'UZ' ? 'Hozircha boshqa donorlar yo\'q' : 'Пока нет других доноров' }}
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue';
import { useI18n } from 'vue-i18n';

const { locale } = useI18n({ useScope: 'global' });
const darkMode = ref(JSON.parse(localStorage.getItem("darkMode")) || false);

const API_BASE = 'https://api.meteordub.uz/api';
const USE_DUMMY_DATA = true; // Set to false when backend is ready

// Dummy data for testing
const dummyDonors = [
  {
    id: 1,
    username: 'Muhammad Ali',
    avatar: 'https://i.pravatar.cc/150?img=12',
    total_amount: 5000000,
    donation_count: 15
  },
  {
    id: 2,
    username: 'Aziza Karimova',
    avatar: 'https://i.pravatar.cc/150?img=45',
    total_amount: 3500000,
    donation_count: 12
  },
  {
    id: 3,
    username: 'Sardor Usmonov',
    avatar: 'https://i.pravatar.cc/150?img=33',
    total_amount: 2800000,
    donation_count: 10
  },
  {
    id: 4,
    username: 'Dilnoza Rashidova',
    avatar: 'https://i.pravatar.cc/150?img=28',
    total_amount: 2100000,
    donation_count: 8
  },
  {
    id: 5,
    username: 'Jahongir Akbarov',
    avatar: 'https://i.pravatar.cc/150?img=52',
    total_amount: 1800000,
    donation_count: 7
  },
  {
    id: 6,
    username: 'Nilufar Hamidova',
    avatar: 'https://i.pravatar.cc/150?img=31',
    total_amount: 1500000,
    donation_count: 6
  },
  {
    id: 7,
    username: 'Bekzod Tursunov',
    avatar: 'https://i.pravatar.cc/150?img=68',
    total_amount: 1200000,
    donation_count: 5
  },
  {
    id: 8,
    username: 'Madina Yusupova',
    avatar: 'https://i.pravatar.cc/150?img=41',
    total_amount: 950000,
    donation_count: 4
  }
];

const donors = ref([]);

const topDonors = computed(() => donors.value.slice(0, 3));
const otherDonors = computed(() => donors.value.slice(3));

function formatAmount(amount) {
  return new Intl.NumberFormat('uz-UZ').format(amount);
}

async function fetchDonors() {
  try {
    if (USE_DUMMY_DATA) {
      // Use dummy data
      donors.value = dummyDonors;
    } else {
      // Fetch from API
      const response = await fetch(`${API_BASE}/donations/`);
      if (!response.ok) throw new Error('Failed to fetch donations');
      const data = await response.json();
      donors.value = data.data || data || [];
    }
  } catch (error) {
    console.error('Error fetching donors:', error);
    // Fallback to dummy data on error
    donors.value = dummyDonors;
  }
}

onMounted(() => {
  if (darkMode.value) document.documentElement.classList.add("dark");
  fetchDonors();
});
</script>

<style scoped>
@keyframes gradient {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}

.animate-gradient {
  background-size: 200% 200%;
  animation: gradient 3s ease infinite;
}

@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-fade-in {
  animation: fade-in 0.6s ease-out forwards;
  opacity: 0;
}

@keyframes slide-up {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.animate-slide-up {
  animation: slide-up 0.8s ease-out forwards;
  opacity: 0;
}

.hover\:scale-115:hover {
  transform: scale(1.15);
}

/* Scrollbar styling */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: transparent;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(to bottom, #a855f7, #ec4899);
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(to bottom, #9333ea, #db2777);
}
</style>