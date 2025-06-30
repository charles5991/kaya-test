<template>
  <div
    class="min-h-screen bg-gradient-to-br from-slate-50 to-slate-100 dark:from-slate-950 dark:to-slate-900"
  >
    <!-- Header -->
    <header
      class="sticky top-0 z-40 bg-white/95 backdrop-blur-sm border-b border-slate-200 dark:bg-slate-900/95 dark:border-slate-800"
    >
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6">
        <div class="flex items-center justify-between">
          <div>
            <h1
              @click="scrollToTop"
              class="text-3xl font-bold text-slate-900 dark:text-white cursor-pointer hover:text-blue-600 dark:hover:text-blue-400 transition-colors"
            >
              Kaya Reports
            </h1>
            <p class="text-slate-600 dark:text-slate-300 mt-1">
              AI-powered insights for performance marketers
            </p>
          </div>
          <div class="flex items-center gap-4">
            <!-- Theme Toggle -->
            <Button
              @click="toggleTheme"
              variant="ghost"
              size="sm"
              class="p-2"
              aria-label="Toggle theme"
            >
              <SunIcon v-if="isDark" class="w-4 h-4" />
              <MoonIcon v-else class="w-4 h-4" />
            </Button>
            <div class="text-sm text-slate-500 dark:text-slate-400">
              {{ new Date().toLocaleDateString() }}
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <div class="mb-8">
        <h2 class="text-2xl font-semibold text-slate-900 dark:text-white mb-2">
          Client Reports
        </h2>
        <p class="text-slate-600 dark:text-slate-300">
          View your latest performance insights and analytics
        </p>
      </div>

      <!-- Loading State -->
      <div
        v-if="loading"
        class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6"
      >
        <div
          v-for="i in 6"
          :key="i"
          class="bg-white dark:bg-slate-800/90 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700/60 p-6 animate-pulse"
        >
          <div
            class="h-4 bg-slate-200 dark:bg-slate-700 rounded w-3/4 mb-4"
          ></div>
          <div
            class="h-3 bg-slate-200 dark:bg-slate-700 rounded w-1/2 mb-2"
          ></div>
          <div class="h-3 bg-slate-200 dark:bg-slate-700 rounded w-2/3"></div>
        </div>
      </div>

      <!-- Reports Grid -->
      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
        <div
          v-for="report in reports"
          :key="report.id"
          :data-report-id="report.id"
          @click="openReport(report)"
          class="bg-white dark:bg-slate-800/90 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700/60 hover:shadow-lg hover:border-slate-300 dark:hover:border-slate-600 dark:hover:bg-slate-800 transition-all duration-200 cursor-pointer group overflow-hidden"
        >
          <!-- Background Image -->
          <div class="relative h-48 overflow-hidden">
            <img
              :src="report.backgroundImageUrl"
              :alt="report.title"
              class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
              @error="handleImageError"
            />
            <div
              class="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent"
            ></div>

            <!-- Tag -->
            <div class="absolute top-4 left-4">
              <span
                class="bg-white/90 dark:bg-slate-800/90 backdrop-blur-sm text-slate-800 dark:text-slate-200 text-xs font-medium px-3 py-1 rounded-full"
              >
                {{ report.tag || "Report" }}
              </span>
            </div>
          </div>

          <!-- Content -->
          <div class="p-6">
            <h3
              class="text-lg font-semibold text-slate-900 dark:text-white mb-2 line-clamp-2"
            >
              {{ report.title }}
            </h3>

            <p
              v-if="report.subtitle"
              class="text-slate-600 dark:text-slate-300 text-sm mb-4 line-clamp-2"
            >
              {{ report.subtitle }}
            </p>

            <div
              class="flex flex-col text-xs text-slate-500 dark:text-slate-400"
            >
              <span v-if="report.datePublished"
                >Published: {{ formatDate(report.datePublished) }}</span
              >
              <span v-if="report.dateUpdated"
                >Updated: {{ formatDate(report.dateUpdated) }}</span
              >
            </div>
          </div>
        </div>
      </div>
    </main>

    <!-- Report Drawer -->
    <Sheet v-model:open="isDrawerOpen" side="right" direction="right">
      <HeroSection v-if="selectedReport" :report="selectedReport" />
    </Sheet>

    <!-- Back to Top Button -->
    <Transition
      enter-active-class="transition-all duration-300 ease-out"
      leave-active-class="transition-all duration-300 ease-in"
      enter-from-class="opacity-0 scale-75 translate-y-4"
      enter-to-class="opacity-100 scale-100 translate-y-0"
      leave-from-class="opacity-100 scale-100 translate-y-0"
      leave-to-class="opacity-0 scale-75 translate-y-4"
    >
      <button
        v-show="showBackToTop"
        @click="scrollToTop"
        class="fixed bottom-6 right-6 z-50 w-12 h-12 bg-blue-600 hover:bg-blue-700 text-white rounded-full shadow-lg hover:shadow-xl transition-all duration-200 flex items-center justify-center group"
        aria-label="Back to top"
      >
        <ChevronUpIcon
          class="w-5 h-5 group-hover:scale-110 transition-transform duration-200"
        />
      </button>
    </Transition>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import Button from "@/components/ui/Button.vue";
import Sheet from "@/components/ui/Sheet.vue";
import HeroSection from "@/components/HeroSection.vue";
import { SunIcon, MoonIcon, ChevronUpIcon } from "lucide-vue-next";

interface Report {
  id: string;
  title: string;
  subtitle?: string;
  backgroundImageUrl: string;
  cta?: string;
  path?: string;
  tag?: string;
  theme?: string;
  datePublished?: string;
  dateUpdated?: string;
}

const reports = ref<Report[]>([]);
const loading = ref(true);
const selectedReport = ref<Report | null>(null);
const isDark = ref(false);
const isDrawerOpen = ref(false);
const showBackToTop = ref(false);

const fetchReports = async () => {
  try {
    const response = await fetch("/reports.json");
    const data = await response.json();
    reports.value = data;
  } catch (error) {
    console.error("Failed to fetch reports:", error);
  } finally {
    loading.value = false;
  }
};

const openReport = (report: Report) => {
  selectedReport.value = report;
  isDrawerOpen.value = true;
};

const toggleTheme = () => {
  isDark.value = !isDark.value;
  if (isDark.value) {
    document.documentElement.classList.add("dark");
    localStorage.setItem("theme", "dark");
  } else {
    document.documentElement.classList.remove("dark");
    localStorage.setItem("theme", "light");
  }
};

const getDummyBackground = (report: Report): string => {
  const backgrounds = {
    "Creative Benchmarking":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="400" height="300" viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="creative" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#667eea;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#764ba2;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="400" height="300" fill="url(#creative)"/>
        <circle cx="100" cy="80" r="40" fill="white" opacity="0.1"/>
        <circle cx="320" cy="200" r="60" fill="white" opacity="0.1"/>
        <rect x="50" y="220" width="80" height="4" fill="white" opacity="0.3"/>
        <rect x="200" y="160" width="120" height="4" fill="white" opacity="0.3"/>
      </svg>
    `),
    "Efficiency Trends":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="400" height="300" viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="efficiency" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#11998e;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#38ef7d;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="400" height="300" fill="url(#efficiency)"/>
        <path d="M50,250 Q150,200 250,180 T350,160" stroke="white" stroke-width="3" fill="none" opacity="0.4"/>
        <circle cx="150" cy="200" r="4" fill="white" opacity="0.6"/>
        <circle cx="250" cy="180" r="4" fill="white" opacity="0.6"/>
        <circle cx="350" cy="160" r="4" fill="white" opacity="0.6"/>
      </svg>
    `),
    "Audience Segments":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="400" height="300" viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="audience" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#fc4a1a;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#f7b733;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="400" height="300" fill="url(#audience)"/>
        <circle cx="120" cy="120" r="25" fill="white" opacity="0.2"/>
        <circle cx="280" cy="120" r="25" fill="white" opacity="0.2"/>
        <circle cx="200" cy="180" r="30" fill="white" opacity="0.2"/>
        <circle cx="150" cy="220" r="20" fill="white" opacity="0.2"/>
        <circle cx="270" cy="220" r="20" fill="white" opacity="0.2"/>
      </svg>
    `),
    "Predictive Insights":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="400" height="300" viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="predictive" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#4568dc;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#b06ab3;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="400" height="300" fill="url(#predictive)"/>
        <path d="M50,150 L100,120 L150,140 L200,100 L250,110 L300,80 L350,90" stroke="white" stroke-width="2" fill="none" opacity="0.4"/>
        <rect x="80" y="240" width="30" height="40" fill="white" opacity="0.2"/>
        <rect x="150" y="220" width="30" height="60" fill="white" opacity="0.2"/>
        <rect x="220" y="200" width="30" height="80" fill="white" opacity="0.2"/>
        <rect x="290" y="180" width="30" height="100" fill="white" opacity="0.2"/>
      </svg>
    `),
    "Creative Hook":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="400" height="300" viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="hook" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#ff9a9e;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#fecfef;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="400" height="300" fill="url(#hook)"/>
        <rect x="50" y="50" width="100" height="60" rx="8" fill="white" opacity="0.2"/>
        <rect x="170" y="80" width="80" height="50" rx="8" fill="white" opacity="0.2"/>
        <rect x="270" y="60" width="90" height="55" rx="8" fill="white" opacity="0.2"/>
        <rect x="100" y="150" width="200" height="80" rx="12" fill="white" opacity="0.3"/>
      </svg>
    `),
    "Performance Index":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="400" height="300" viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="performance" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#2196F3;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#21CBF3;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="400" height="300" fill="url(#performance)"/>
        <circle cx="200" cy="150" r="80" stroke="white" stroke-width="3" fill="none" opacity="0.3"/>
        <circle cx="200" cy="150" r="50" stroke="white" stroke-width="2" fill="none" opacity="0.4"/>
        <circle cx="200" cy="150" r="20" fill="white" opacity="0.5"/>
        <text x="200" y="260" text-anchor="middle" fill="white" opacity="0.6" font-family="system-ui" font-size="14">Performance</text>
      </svg>
    `),
    default:
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="400" height="300" viewBox="0 0 400 300" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="default" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#6366f1;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#8b5cf6;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="400" height="300" fill="url(#default)"/>
        <rect x="50" y="100" width="300" height="100" rx="12" fill="white" opacity="0.1"/>
        <circle cx="100" cy="150" r="30" fill="white" opacity="0.2"/>
        <rect x="150" y="130" width="180" height="8" fill="white" opacity="0.3"/>
        <rect x="150" y="150" width="120" height="8" fill="white" opacity="0.3"/>
        <rect x="150" y="170" width="200" height="8" fill="white" opacity="0.3"/>
      </svg>
    `),
  };

  return (
    backgrounds[report.theme as keyof typeof backgrounds] || backgrounds.default
  );
};

const handleImageError = (event: Event) => {
  const img = event.target as HTMLImageElement;
  const reportElement = img.closest("[data-report-id]");
  const reportId = reportElement?.getAttribute("data-report-id");
  const report = reports.value.find((r) => r.id === reportId);

  if (report) {
    img.src = getDummyBackground(report);
  } else {
    img.src = getDummyBackground({ theme: "default" } as Report);
  }
};

const formatDate = (dateString: string): string => {
  try {
    return new Date(dateString).toLocaleDateString();
  } catch {
    return dateString;
  }
};

const scrollToTop = () => {
  window.scrollTo({
    top: 0,
    behavior: "smooth",
  });
};

const handleScroll = () => {
  showBackToTop.value = window.scrollY > 300;
};

onMounted(() => {
  fetchReports();

  // Initialize theme
  const savedTheme = localStorage.getItem("theme");

  // Default to light mode for now, but respect user preference if set
  if (savedTheme === "dark") {
    isDark.value = true;
    document.documentElement.classList.add("dark");
  } else if (savedTheme === "light") {
    isDark.value = false;
    document.documentElement.classList.remove("dark");
  } else {
    // Default to light mode
    isDark.value = false;
    document.documentElement.classList.remove("dark");
    localStorage.setItem("theme", "light");
  }

  window.addEventListener("scroll", handleScroll);
});

onUnmounted(() => {
  window.removeEventListener("scroll", handleScroll);
});
</script>

<style scoped>
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>
