<script setup lang="ts">
import { ref, onMounted } from "vue";
import Image1 from "./assets/images/photo1.jpeg";
import Image2 from "./assets/images/photo2.jpeg";
import Image3 from "./assets/images/photo3.jpeg";
import Image4 from "./assets/images/photo4.jpeg";

// Screen navigation
const currentScreen = ref(0);
const isTransitioning = ref(false);

// Screen 1: Opening
const showOpening = ref(false);

// Screen 2: Animated Quotes
const showQuotes = ref(false);
const currentQuote = ref(0);
const quotes = [
  {
    text: "Every moment with you feels like a beautiful dream I never want to wake up from.",
    emoji: "✨",
  },
  {
    text: "You're not just a year older, you're a year more wonderful, more precious, more loved.",
    emoji: "💖",
  },
  {
    text: "In a world of temporary things, you are my constant, my forever.",
    emoji: "🌟",
  },
];

// Screen 3: Countdown
const countdown = ref(3);
const showCountdown = ref(false);

// Screen 4: Birthday Reveal
const showBirthdayReveal = ref(false);

// Screen 5: Why I Love You
const showWhyLoveYou = ref(false);
const currentReason = ref(0);
const reasons = [
  {
    title: "Your Smile",
    text: "That lights up even my darkest days and makes everything worth it",
    icon: "😊",
  },
  {
    title: "Your Kindness",
    text: "The way you care for others shows the beautiful soul you have",
    icon: "💝",
  },
  {
    title: "Your Strength",
    text: "You face every challenge with grace and courage that inspires me",
    icon: "💪",
  },
  {
    title: "Your Laughter",
    text: "The most beautiful sound that fills my heart with pure joy",
    icon: "😄",
  },
  {
    title: "Everything About You",
    text: "Every little thing makes you perfectly imperfect, and I love it all",
    icon: "💕",
  },
];

// Screen 6: Photo Gallery
const showGallery = ref(false);
const currentPhoto = ref(0);

// Screen 7 & 8: Love Messages
const showMessage1 = ref(false);
const showMessage2 = ref(false);

// Screen 9: Wishes
const showWishes = ref(false);

// Screen 10: Final
const showFinal = ref(false);

const balloons = ref<
  Array<{ id: number; color: string; delay: number; left: string }>
>([]);

onMounted(() => {
  // Generate random balloons
  const colors = [
    "bg-pink-400",
    "bg-purple-400",
    "bg-blue-400",
    "bg-yellow-400",
    "bg-rose-400",
    "bg-indigo-400",
  ];
  for (let i = 0; i < 30; i++) {
    balloons.value.push({
      id: i,
      color: colors[Math.floor(Math.random() * colors.length)] || "bg-pink-400",
      delay: Math.random() * 5,
      left: `${Math.random() * 100}%`,
    });
  }

  // Start with opening screen
  setTimeout(() => {
    showOpening.value = true;
  }, 300);
});

const images = [Image1, Image2, Image3, Image4];

const nextScreen = () => {
  if (isTransitioning.value) return;

  isTransitioning.value = true;

  setTimeout(() => {
    currentScreen.value++;
    isTransitioning.value = false;

    // Trigger animations for each screen
    if (currentScreen.value === 1) {
      setTimeout(() => (showQuotes.value = true), 300);
      autoPlayQuotes();
    } else if (currentScreen.value === 2) {
      startCountdown();
    } else if (currentScreen.value === 3) {
      setTimeout(() => (showBirthdayReveal.value = true), 300);
    } else if (currentScreen.value === 4) {
      setTimeout(() => (showWhyLoveYou.value = true), 300);
      autoPlayReasons();
    } else if (currentScreen.value === 5) {
      setTimeout(() => (showGallery.value = true), 300);
    } else if (currentScreen.value === 6) {
      setTimeout(() => (showMessage1.value = true), 300);
    } else if (currentScreen.value === 7) {
      setTimeout(() => (showMessage2.value = true), 300);
    } else if (currentScreen.value === 8) {
      setTimeout(() => (showWishes.value = true), 300);
    } else if (currentScreen.value === 9) {
      setTimeout(() => (showFinal.value = true), 300);
    }
  }, 500);
};

const autoPlayQuotes = () => {
  const interval = setInterval(() => {
    if (currentQuote.value < quotes.length - 1) {
      currentQuote.value++;
    } else {
      clearInterval(interval);
    }
  }, 5000);
};

const autoPlayReasons = () => {
  const interval = setInterval(() => {
    if (currentReason.value < reasons.length - 1) {
      currentReason.value++;
    } else {
      clearInterval(interval);
    }
  }, 5000);
};

const startCountdown = () => {
  showCountdown.value = true;
  countdown.value = 3;

  const interval = setInterval(() => {
    countdown.value--;
    if (countdown.value === 0) {
      clearInterval(interval);
      setTimeout(() => {
        nextScreen();
      }, 800);
    }
  }, 1000);
};

const nextPhoto = () => {
  if (currentPhoto.value < images.length - 1) {
    currentPhoto.value++;
  } else {
    nextScreen();
  }
};

const prevPhoto = () => {
  if (currentPhoto.value > 0) {
    currentPhoto.value--;
  }
};

const nextReason = () => {
  if (currentReason.value < reasons.length - 1) {
    currentReason.value++;
  } else {
    nextScreen();
  }
};

const prevReason = () => {
  if (currentReason.value > 0) {
    currentReason.value--;
  }
};
</script>

<template>
  <div class="min-h-screen relative overflow-hidden">
    <!-- Animated Background Balloons -->
    <div
      class="absolute inset-0 pointer-events-none overflow-hidden"
      v-if="currentScreen >= 2"
    >
      <div
        v-for="balloon in balloons"
        :key="balloon.id"
        class="absolute bottom-0 w-8 h-10 rounded-full opacity-60"
        :class="balloon.color"
        :style="{
          left: balloon.left,
          animationDelay: `${balloon.delay}s`,
          animation: 'float 8s ease-in-out infinite',
        }"
      >
        <div class="w-0.5 h-12 bg-gray-400 mx-auto"></div>
      </div>
    </div>

    <!-- Screen 0: Opening -->
    <div
      v-if="currentScreen === 0"
      class="min-h-screen bg-linear-to-br from-indigo-900 via-purple-900 to-pink-900 flex items-center justify-center transition-opacity duration-1000"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <div v-if="showOpening" class="text-center animate-fadeIn px-4">
        <div class="text-6xl mb-8 animate-float">🎁</div>
        <h1
          class="text-5xl md:text-7xl font-bold text-white mb-8 animate-pulse-custom"
        >
          Ada Sesuatu Untukmu...
        </h1>
        <p class="text-2xl md:text-3xl text-pink-200 mb-12 italic">
          Someone special...
        </p>
        <button
          @click="nextScreen"
          class="bg-white text-purple-900 px-12 py-4 rounded-full text-2xl font-bold hover:scale-110 transition-transform duration-300 shadow-2xl hover:shadow-pink-500/50 cursor-pointer"
        >
          Buka 💝
        </button>
      </div>
    </div>

    <!-- Screen 1: Animated Quotes -->
    <div
      v-if="currentScreen === 1"
      class="min-h-screen bg-linear-to-br from-violet-900 via-fuchsia-900 to-pink-900 flex items-center justify-center transition-opacity duration-1000 relative overflow-hidden"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <!-- Floating stars -->
      <div class="absolute inset-0 pointer-events-none">
        <div
          v-for="i in 15"
          :key="i"
          class="absolute text-yellow-200 text-2xl animate-sparkle"
          :style="{
            top: `${Math.random() * 100}%`,
            left: `${Math.random() * 100}%`,
            animationDelay: `${Math.random() * 2}s`,
          }"
        >
          ✨
        </div>
      </div>

      <div v-if="showQuotes" class="max-w-4xl mx-auto px-4 relative z-10">
        <div
          v-if="quotes[currentQuote]"
          :key="`quote-${currentQuote}`"
          class="text-center animate-fadeIn"
        >
          <div class="text-8xl mb-8 animate-pulse-custom">
            {{ quotes[currentQuote]?.emoji }}
          </div>
          <p
            class="text-3xl md:text-5xl text-white font-serif italic leading-relaxed mb-12"
          >
            "{{ quotes[currentQuote]?.text }}"
          </p>
        </div>

        <div class="text-center mt-12">
          <button
            v-if="currentQuote === quotes.length - 1"
            @click="nextScreen"
            class="bg-white text-purple-900 px-10 py-4 rounded-full text-xl font-bold hover:scale-110 transition-transform duration-300 shadow-2xl cursor-pointer"
          >
            Continue ✨
          </button>
          <div v-else class="flex justify-center gap-2">
            <div
              v-for="(_, index) in quotes"
              :key="index"
              class="w-3 h-3 rounded-full transition-all duration-300"
              :class="currentQuote === index ? 'bg-white w-8' : 'bg-white/30'"
            ></div>
          </div>
        </div>
      </div>
    </div>

    <!-- Screen 2: Countdown -->
    <div
      v-if="currentScreen === 2"
      class="min-h-screen bg-linear-to-br from-purple-900 via-pink-900 to-rose-900 flex items-center justify-center transition-opacity duration-1000"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <div v-if="showCountdown" class="text-center animate-fadeIn">
        <div
          v-if="countdown > 0"
          class="text-9xl md:text-[20rem] font-bold text-white animate-pulse-custom"
        >
          {{ countdown }}
        </div>
        <div v-else class="text-7xl md:text-9xl animate-sparkle">🎉✨🎊</div>
      </div>
    </div>

    <!-- Screen 3: Birthday Reveal -->
    <div
      v-if="currentScreen === 3"
      class="min-h-screen bg-linear-to-br from-pink-50 via-purple-50 to-blue-50 flex items-center justify-center transition-opacity duration-1000 relative"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <!-- Sparkles -->
      <div class="absolute inset-0 pointer-events-none">
        <div
          class="absolute top-20 left-10 text-yellow-400 text-4xl animate-sparkle"
        >
          ✨
        </div>
        <div
          class="absolute top-40 right-20 text-pink-400 text-5xl animate-sparkle"
          style="animation-delay: 0.5s"
        >
          💖
        </div>
        <div
          class="absolute bottom-32 left-20 text-purple-400 text-4xl animate-sparkle"
          style="animation-delay: 1s"
        >
          ⭐
        </div>
        <div
          class="absolute top-1/3 right-10 text-blue-400 text-4xl animate-sparkle"
          style="animation-delay: 1.5s"
        >
          ✨
        </div>
        <div
          class="absolute bottom-20 right-40 text-rose-400 text-5xl animate-sparkle"
          style="animation-delay: 0.8s"
        >
          💝
        </div>
      </div>

      <div
        v-if="showBirthdayReveal"
        class="text-center animate-fadeIn px-4 relative z-10"
      >
        <div class="mb-8">
          <h1
            class="text-8xl md:text-[12rem] font-bold bg-linear-to-r from-pink-500 via-purple-500 to-blue-500 bg-clip-text text-transparent mb-4 animate-pulse-custom"
          >
            24
          </h1>
          <div class="text-7xl md:text-9xl mb-6 animate-float">🎂🎉</div>
        </div>
        <h2
          class="text-5xl md:text-7xl font-bold text-purple-600 mb-6 animate-float"
          style="animation-delay: 0.3s"
        >
          Happy Birthday
        </h2>
        <h3
          class="text-6xl md:text-8xl font-bold bg-linear-to-r from-rose-500 to-pink-500 bg-clip-text text-transparent mb-8 animate-pulse-custom"
          style="animation-delay: 0.5s"
        >
          Hanifa ❤️💕
        </h3>
        <p
          class="text-2xl md:text-4xl text-pink-600 font-semibold italic mb-12"
        >
          Selamat ulang tahun Pretyyyyyyyy!!!!! 🌸
        </p>
        <button
          @click="nextScreen"
          class="bg-linear-to-r from-pink-500 to-purple-500 text-white px-10 py-4 rounded-full text-xl font-bold hover:scale-110 transition-transform duration-300 shadow-2xl cursor-pointer"
        >
          Lanjut 💕
        </button>
      </div>
    </div>

    <!-- Screen 4: Why I Love You -->
    <div
      v-if="currentScreen === 4"
      class="min-h-screen bg-linear-to-br from-rose-900 via-pink-900 to-purple-900 flex items-center justify-center transition-opacity duration-5000 p-4 relative overflow-hidden"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <!-- Floating hearts background -->
      <div class="absolute inset-0 pointer-events-none">
        <div
          v-for="i in 20"
          :key="i"
          class="absolute text-pink-300 opacity-20 animate-float"
          :style="{
            top: `${Math.random() * 100}%`,
            left: `${Math.random() * 100}%`,
            fontSize: `${Math.random() * 40 + 20}px`,
            animationDelay: `${Math.random() * 3}s`,
          }"
        >
          💕
        </div>
      </div>

      <div v-if="showWhyLoveYou" class="max-w-4xl mx-auto relative z-10">
        <h2
          class="text-5xl md:text-6xl font-bold text-center text-white mb-16 animate-fadeIn"
        >
          Why I Love You 💝
        </h2>

        <div
          v-if="reasons[currentReason]"
          :key="`reason-${currentReason}`"
          class="animate-fadeIn"
        >
          <div
            class="bg-white/10 backdrop-blur-lg rounded-3xl shadow-2xl p-12 border-2 border-white/20"
          >
            <div class="text-center mb-8">
              <div class="text-9xl mb-6 animate-pulse-custom">
                {{ reasons[currentReason]?.icon }}
              </div>
              <h3 class="text-4xl md:text-5xl font-bold text-white mb-6">
                {{ reasons[currentReason]?.title }}
              </h3>
              <p
                class="text-2xl md:text-3xl text-pink-100 leading-relaxed italic"
              >
                {{ reasons[currentReason]?.text }}
              </p>
            </div>
          </div>
        </div>

        <div class="flex justify-center gap-4 mt-12">
          <button
            v-if="currentReason > 0"
            @click="prevReason"
            class="bg-white/20 text-white px-8 py-3 rounded-full text-lg font-bold hover:scale-110 transition-transform duration-300 backdrop-blur-sm cursor-pointer"
          >
            ← Back
          </button>
          <button
            @click="nextReason"
            class="bg-white text-purple-900 px-8 py-3 rounded-full text-lg font-bold hover:scale-110 transition-transform duration-300 shadow-2xl cursor-pointer"
          >
            {{
              currentReason === reasons.length - 1 ? "Continue 💕" : "Next →"
            }}
          </button>
        </div>

        <div class="flex justify-center gap-2 mt-8">
          <div
            v-for="(_, index) in reasons"
            :key="index"
            class="w-3 h-3 rounded-full transition-all duration-300"
            :class="currentReason === index ? 'bg-white w-8' : 'bg-white/30'"
          ></div>
        </div>
      </div>
    </div>

    <!-- Screen 5: Photo Gallery -->
    <div
      v-if="currentScreen === 5"
      class="min-h-screen bg-linear-to-br from-rose-50 via-pink-50 to-purple-50 flex items-center justify-center transition-opacity duration-1000 p-4"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <div v-if="showGallery" class="max-w-4xl w-full animate-fadeIn">
        <h2
          class="text-4xl md:text-5xl font-bold text-center text-purple-600 mb-8"
        >
          Moments with You 💝
        </h2>

        <div class="relative">
          <!-- Main Photo -->
          <div
            class="relative overflow-hidden rounded-3xl shadow-2xl mb-6 animate-pulse-custom"
          >
            <img
              :src="images[currentPhoto]"
              :alt="`Memory ${currentPhoto + 1}`"
              class="w-full h-[500px] object-cover"
            />
            <div
              class="absolute inset-0 bg-linear-to-t from-purple-900/50 to-transparent flex items-end justify-center pb-8"
            >
              <span class="text-white text-3xl font-bold"
                >{{ currentPhoto + 1 }} / {{ images.length }}</span
              >
            </div>
          </div>

          <!-- Navigation -->
          <div class="flex justify-center gap-4 mb-6">
            <button
              @click="prevPhoto"
              :disabled="currentPhoto === 0"
              class="bg-purple-500 text-white px-8 py-3 rounded-full text-lg font-bold hover:scale-110 transition-transform duration-300 disabled:opacity-50 disabled:cursor-not-allowed cursor-pointer"
            >
              ← Previous
            </button>
            <button
              @click="nextPhoto"
              class="bg-pink-500 text-white px-8 py-3 rounded-full text-lg font-bold hover:scale-110 transition-transform duration-300 cursor-pointer"
            >
              {{
                currentPhoto === images.length - 1 ? "Continue ✨" : "Next →"
              }}
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Screen 6: Love Message 1 -->
    <div
      v-if="currentScreen === 6"
      class="min-h-screen bg-linear-to-br from-purple-50 via-pink-50 to-rose-50 flex items-center justify-center transition-opacity duration-1000 p-4"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <div v-if="showMessage1" class="max-w-4xl mx-auto animate-slideInLeft">
        <div
          class="bg-white/70 backdrop-blur-md rounded-3xl shadow-2xl p-8 md:p-12 border-4 border-pink-200 relative"
        >
          <div class="absolute -top-8 -left-8 text-8xl animate-float">💌</div>
          <div
            class="absolute -bottom-8 -right-8 text-8xl animate-float"
            style="animation-delay: 0.5s"
          >
            💝
          </div>

          <p
            class="text-xl md:text-2xl text-gray-800 leading-relaxed text-center font-serif italic mb-12"
          >
            <span class="text-purple-600 font-bold text-4xl">"</span>
            <br />
            True love is not merely a feeling; it is a living promise.
            <br /><br />
            It is the compass that guides you through life's uncharted paths,
            the calm that stills the storm within, and the anchor that holds you
            fast when the tides threaten to pull you away.
            <br /><br />
            It is a silent, steadfast vow to stand beside one another — in every
            season, through every trial, come what may.
            <br />
            <span class="text-purple-600 font-bold text-4xl">"</span>
          </p>

          <div class="text-center">
            <button
              @click="nextScreen"
              class="bg-linear-to-r from-purple-500 to-pink-500 text-white px-10 py-4 rounded-full text-xl font-bold hover:scale-110 transition-transform duration-300 shadow-2xl cursor-pointer"
            >
              Continue 💕
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Screen 7: Love Message 2 -->
    <div
      v-if="currentScreen === 7"
      class="min-h-screen bg-linear-to-br from-blue-50 via-purple-50 to-pink-50 flex items-center justify-center transition-opacity duration-1000 p-4"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <div v-if="showMessage2" class="max-w-4xl mx-auto animate-slideInRight">
        <div
          class="bg-linear-to-br from-pink-100 via-purple-100 to-blue-100 rounded-3xl shadow-2xl p-8 md:p-12 border-4 border-purple-300 relative"
        >
          <div class="absolute -top-8 -right-8 text-8xl animate-pulse-custom">
            💖
          </div>
          <div
            class="absolute -bottom-8 -left-8 text-8xl animate-pulse-custom"
            style="animation-delay: 0.5s"
          >
            🌹
          </div>

          <div
            class="text-xl md:text-2xl text-gray-800 leading-relaxed text-center font-serif italic space-y-6 mb-12"
          >
            <p class="text-pink-600 font-semibold text-2xl">
              When completely immersed in some ideas;<br />I'm making some
              childish wishes…
            </p>
            <p class="text-purple-600 font-bold text-3xl">
              Please stay with me.<br />Be with me, please....
            </p>
            <p class="text-blue-600 text-xl">
              Don't be mad at my heart for being silly....
            </p>
            <p class="text-rose-600 italic text-xl">
              Like a message in the eyes,<br />like prayers uttered from a
              mosque....
            </p>
          </div>

          <div class="text-center">
            <button
              @click="nextScreen"
              class="bg-linear-to-r from-pink-500 to-rose-500 text-white px-10 py-4 rounded-full text-xl font-bold hover:scale-110 transition-transform duration-300 shadow-2xl cursor-pointer"
            >
              Continue 🌹
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Screen 8: Birthday Wishes -->
    <div
      v-if="currentScreen === 8"
      class="min-h-screen bg-linear-to-br from-amber-50 via-orange-50 to-pink-50 flex items-center justify-center transition-opacity duration-1000 p-4 relative overflow-hidden"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <!-- Confetti effect -->
      <div class="absolute inset-0 pointer-events-none">
        <div
          v-for="i in 30"
          :key="i"
          class="absolute text-3xl animate-float opacity-80"
          :style="{
            top: `${Math.random() * 100}%`,
            left: `${Math.random() * 100}%`,
            animationDelay: `${Math.random() * 2}s`,
          }"
        >
          {{
            ["🎈", "🎉", "🎊", "✨", "💫", "⭐"][Math.floor(Math.random() * 6)]
          }}
        </div>
      </div>

      <div v-if="showWishes" class="max-w-5xl mx-auto relative z-10">
        <div class="text-center mb-12 animate-fadeIn">
          <div class="text-8xl mb-6 animate-pulse-custom">🎂</div>
          <h2
            class="text-5xl md:text-6xl font-bold text-transparent bg-linear-to-r from-orange-500 via-pink-500 to-purple-500 bg-clip-text mb-8"
          >
            My Wishes For You
          </h2>
        </div>

        <div class="grid md:grid-cols-2 gap-6 mb-12">
          <div
            class="bg-white/70 backdrop-blur-md rounded-2xl p-8 shadow-xl border-2 border-pink-200 animate-slideInLeft"
          >
            <div class="text-5xl mb-4">🌟</div>
            <h3 class="text-2xl font-bold text-purple-600 mb-3">Happiness</h3>
            <p class="text-lg text-gray-700">
              May every day be filled with laughter, joy, and moments that make
              your heart smile
            </p>
          </div>

          <div
            class="bg-white/70 backdrop-blur-md rounded-2xl p-8 shadow-xl border-2 border-purple-200 animate-slideInRight"
            style="animation-delay: 0.2s"
          >
            <div class="text-5xl mb-4">💪</div>
            <h3 class="text-2xl font-bold text-pink-600 mb-3">Strength</h3>
            <p class="text-lg text-gray-700">
              The courage to face challenges and the wisdom to overcome them all
            </p>
          </div>

          <div
            class="bg-white/70 backdrop-blur-md rounded-2xl p-8 shadow-xl border-2 border-blue-200 animate-slideInLeft"
            style="animation-delay: 0.4s"
          >
            <div class="text-5xl mb-4">✨</div>
            <h3 class="text-2xl font-bold text-blue-600 mb-3">
              Dreams Come True
            </h3>
            <p class="text-lg text-gray-700">
              May all your dreams and aspirations manifest into beautiful
              reality
            </p>
          </div>

          <div
            class="bg-white/70 backdrop-blur-md rounded-2xl p-8 shadow-xl border-2 border-rose-200 animate-slideInRight"
            style="animation-delay: 0.6s"
          >
            <div class="text-5xl mb-4">💝</div>
            <h3 class="text-2xl font-bold text-rose-600 mb-3">Love & Peace</h3>
            <p class="text-lg text-gray-700">
              Surrounded by love, filled with peace, and blessed beyond measure
            </p>
          </div>
        </div>

        <div class="text-center animate-fadeIn" style="animation-delay: 0.8s">
          <button
            @click="nextScreen"
            class="bg-linear-to-r from-orange-500 via-pink-500 to-purple-500 text-white px-12 py-4 rounded-full text-xl font-bold hover:scale-110 transition-transform duration-300 shadow-2xl cursor-pointer"
          >
            One More Thing... 💕
          </button>
        </div>
      </div>
    </div>

    <!-- Screen 9: Final Message -->
    <div
      v-if="currentScreen === 9"
      class="min-h-screen bg-linear-to-br from-pink-900 via-purple-900 to-blue-900 flex items-center justify-center transition-opacity duration-1000 relative overflow-hidden"
      :class="{ 'opacity-0': isTransitioning }"
    >
      <!-- Floating hearts -->
      <div class="absolute inset-0 pointer-events-none">
        <div class="absolute top-10 left-10 text-6xl animate-float opacity-70">
          💕
        </div>
        <div
          class="absolute top-20 right-20 text-5xl animate-float opacity-70"
          style="animation-delay: 0.5s"
        >
          💖
        </div>
        <div
          class="absolute bottom-20 left-20 text-7xl animate-float opacity-70"
          style="animation-delay: 1s"
        >
          💝
        </div>
        <div
          class="absolute bottom-32 right-32 text-6xl animate-float opacity-70"
          style="animation-delay: 1.5s"
        >
          💗
        </div>
        <div
          class="absolute top-1/2 left-1/4 text-5xl animate-float opacity-70"
          style="animation-delay: 0.8s"
        >
          💓
        </div>
      </div>

      <div
        v-if="showFinal"
        class="text-center animate-fadeIn px-4 relative z-10"
      >
        <div class="text-8xl mb-8 animate-pulse-custom">💝</div>
        <h1
          class="text-5xl md:text-7xl font-bold text-white mb-8 leading-tight"
        >
          With All My Love
        </h1>
        <div class="text-8xl md:text-9xl mb-8 animate-float">💕</div>
        <p class="text-2xl md:text-3xl text-pink-200 mb-8 italic">
          For Hanifa, on your special 24th birthday
        </p>
        <div class="text-6xl animate-float mb-12">🎈🎊🎁🎀✨</div>
        <p class="text-xl md:text-2xl text-purple-200 font-semibold">
          May this year bring you endless joy, love, and beautiful moments 🌸
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Additional custom animations handled in style.css */
</style>
