<template>
  <div
    class="min-h-screen flex items-center justify-center bg-cover bg-center relative p-4 overflow-hidden"
    style="background-image: url('/v2.jpg')"
  >
    <!-- Subtle overlay to enhance readability -->
    <div class="absolute inset-0 bg-black/20"></div>

    <!-- Signup Card -->
    <div class="relative z-10 w-full max-w-md">
      <div class="backdrop-blur-xl bg-white/5 rounded-3xl shadow-2xl border border-white/10 p-8 sm:p-10 transform transition-all duration-500 hover:scale-[1.02] hover:border-white/20">
        <!-- Logo/Icon -->
        <div class="flex justify-center mb-6">
          <div class="w-16 h-16 sm:w-20 sm:h-20 bg-gradient-to-br from-purple-500 to-pink-600 rounded-2xl flex items-center justify-center shadow-lg shadow-purple-500/30 transform transition-transform hover:rotate-12">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="w-8 h-8 sm:w-10 sm:h-10 text-white"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M18 9v3m0 0v3m0-3h3m-3 0h-3m-2-5a4 4 0 11-8 0 4 4 0 018 0zM3 20a6 6 0 0112 0v1H3v-1z" />
            </svg>
          </div>
        </div>

        <!-- Title -->
        <h2 class="text-3xl sm:text-4xl font-bold text-center mb-2 text-white drop-shadow-lg">
          Ro'yxatdan o'tish
        </h2>
        <p class="text-center text-white/70 mb-8 text-sm sm:text-base">
          Hisobingizni yaratish uchun ma'lumotlarni kiriting
        </p>

        <!-- Form -->
        <form @submit.prevent="handleSignup" class="space-y-5">
          <!-- Name Input -->
          <div class="group">
            <label class="block text-sm font-medium mb-2 text-white/90">
              Ism
            </label>
            <div class="relative">
              <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 text-white/50 group-focus-within:text-white/80 transition-colors"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
                </svg>
              </div>
              <input
                v-model="name"
                type="text"
                required
                class="w-full pl-12 pr-4 py-3 sm:py-3.5 rounded-2xl bg-white/5 text-white placeholder-white/40 focus:outline-none focus:ring-2 focus:ring-purple-400 focus:bg-white/10 transition-all duration-300 text-sm sm:text-base border border-white/10 focus:border-purple-400/50"
                placeholder="Ismingiz"
              />
            </div>
          </div>

          <!-- Email Input -->
          <div class="group">
            <label class="block text-sm font-medium mb-2 text-white/90">
              Email
            </label>
            <div class="relative">
              <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 text-white/50 group-focus-within:text-white/80 transition-colors"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 12a4 4 0 10-8 0 4 4 0 008 0zm0 0v1.5a2.5 2.5 0 005 0V12a9 9 0 10-9 9m4.5-1.206a8.959 8.959 0 01-4.5 1.207" />
                </svg>
              </div>
              <input
                v-model="email"
                type="email"
                required
                class="w-full pl-12 pr-4 py-3 sm:py-3.5 rounded-2xl bg-white/5 text-white placeholder-white/40 focus:outline-none focus:ring-2 focus:ring-purple-400 focus:bg-white/10 transition-all duration-300 text-sm sm:text-base border border-white/10 focus:border-purple-400/50"
                placeholder="example@email.com"
              />
            </div>
          </div>

          <!-- Password Input -->
          <div class="group">
            <label class="block text-sm font-medium mb-2 text-white/90">
              Parol
            </label>
            <div class="relative">
              <div class="absolute inset-y-0 left-0 pl-4 flex items-center pointer-events-none">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5 text-white/50 group-focus-within:text-white/80 transition-colors"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" />
                </svg>
              </div>
              <input
                v-model="password"
                :type="showPassword ? 'text' : 'password'"
                required
                class="w-full pl-12 pr-12 py-3 sm:py-3.5 rounded-2xl bg-white/5 text-white placeholder-white/40 focus:outline-none focus:ring-2 focus:ring-purple-400 focus:bg-white/10 transition-all duration-300 text-sm sm:text-base border border-white/10 focus:border-purple-400/50"
                placeholder="••••••••"
              />
              <button
                type="button"
                @click="showPassword = !showPassword"
                class="absolute inset-y-0 right-0 pr-4 flex items-center text-white/50 hover:text-white/80 transition-colors"
              >
                <svg
                  v-if="!showPassword"
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z" />
                </svg>
                <svg
                  v-else
                  xmlns="http://www.w3.org/2000/svg"
                  class="h-5 w-5"
                  fill="none"
                  viewBox="0 0 24 24"
                  stroke="currentColor"
                >
                  <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.875 18.825A10.05 10.05 0 0112 19c-4.478 0-8.268-2.943-9.543-7a9.97 9.97 0 011.563-3.029m5.858.908a3 3 0 114.243 4.243M9.878 9.878l4.242 4.242M9.88 9.88l-3.29-3.29m7.532 7.532l3.29 3.29M3 3l3.59 3.59m0 0A9.953 9.953 0 0112 5c4.478 0 8.268 2.943 9.543 7a10.025 10.025 0 01-4.132 5.411m0 0L21 21" />
                </svg>
              </button>
            </div>
          </div>

          <!-- Error Message -->
          <transition name="fade">
            <div
              v-if="error"
              class="bg-red-500/20 border border-red-500/50 rounded-2xl p-3 flex items-center gap-2 animate-shake"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="h-5 w-5 text-red-300 flex-shrink-0"
                fill="none"
                viewBox="0 0 24 24"
                stroke="currentColor"
              >
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4m0 4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
              <p class="text-red-200 text-sm font-medium">{{ error }}</p>
            </div>
          </transition>

          <!-- Submit Button -->
          <button
            type="submit"
            :disabled="isLoading"
            class="w-full py-3.5 mt-2 bg-gradient-to-r from-purple-500 via-pink-500 to-rose-500 hover:from-purple-600 hover:via-pink-600 hover:to-rose-600 text-white font-semibold rounded-2xl transition-all duration-300 text-base shadow-lg shadow-purple-500/50 hover:shadow-purple-500/70 hover:scale-[1.02] disabled:opacity-50 disabled:cursor-not-allowed flex items-center justify-center gap-2"
          >
            <svg
              v-if="isLoading"
              class="animate-spin h-5 w-5 text-white"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
            >
              <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
              <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
            </svg>
            <span>{{ isLoading ? "Ro'yxatdan o'tilmoqda..." : "Ro'yxatdan o'tish" }}</span>
          </button>
        </form>

        <!-- Divider -->
        <div class="relative my-6">
          <div class="absolute inset-0 flex items-center">
            <div class="w-full border-t border-white/20"></div>
          </div>
          <div class="relative flex justify-center text-sm">
            <span class="px-4 bg-transparent text-white/60">yoki</span>
          </div>
        </div>

        <!-- Login Link -->
        <div class="text-center">
          <p class="text-white/70 text-sm mb-3">Allaqachon hisobingiz bormi?</p>
          <router-link
            to="/login"
            class="inline-flex items-center gap-2 px-6 py-2.5 bg-white/10 hover:bg-white/20 text-white font-medium rounded-2xl transition-all duration-300 text-sm border border-white/20 hover:border-white/40 hover:scale-105"
          >
            <span>Tizimga kirish</span>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="h-4 w-4"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 7l5 5m0 0l-5 5m5-5H6" />
            </svg>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import { signup } from "../utils/auth";

const name = ref("");
const email = ref("");
const password = ref("");
const showPassword = ref(false);
const error = ref(null);
const isLoading = ref(false);
const router = useRouter();

const handleSignup = async () => {
  error.value = null;
  isLoading.value = true;

  const res = await signup({
    name: name.value,
    email: email.value,
    password: password.value,
  });

  isLoading.value = false;

  if (!res.ok) {
    // Check for specific email error and translate to Uzbek
    if (res.errors && res.errors.email && res.errors.email.length > 0) {
      const emailError = res.errors.email[0];
      if (emailError === "user with this email already exists.") {
        error.value = "Bu email allaqachon band";
      } else {
        error.value = emailError;
      }
    } else {
      error.value = res.message || "Xatolik yuz berdi. Iltimos, qayta urinib ko'ring.";
    }
  } else {
    await new Promise(resolve => setTimeout(resolve, 50));
    router.push({ name: 'Home' });
  }
};
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-6px);
}

@keyframes shake {
  0%, 100% {
    transform: translateX(0);
  }
  25% {
    transform: translateX(-5px);
  }
  75% {
    transform: translateX(5px);
  }
}

.animate-shake {
  animation: shake 0.3s ease-in-out;
}
</style>