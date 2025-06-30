<template>
  <div class="relative overflow-hidden">
    <!-- Background Image -->
    <div class="absolute inset-0">
      <img
        :src="report.backgroundImageUrl"
        :alt="report.title"
        class="w-full h-full object-cover filter blur-sm"
        @error="handleImageError"
      />
      <div
        class="absolute inset-0 bg-gradient-to-r from-black/85 via-black/75 to-black/65"
      ></div>
    </div>

    <!-- Content -->
    <div class="relative z-10 px-6 py-8 lg:px-8 lg:py-12">
      <div class="max-w-4xl">
        <!-- Tag and Theme -->
        <div class="flex flex-wrap gap-2 mb-6">
          <span
            v-if="report.tag"
            class="bg-white/20 backdrop-blur-sm text-white text-xs font-medium px-3 py-1.5 rounded-full"
          >
            {{ report.tag }}
          </span>
          <span
            v-if="report.theme"
            class="bg-blue-500/20 backdrop-blur-sm text-blue-100 text-xs font-medium px-3 py-1.5 rounded-full"
          >
            {{ report.theme }}
          </span>
        </div>

        <!-- Title -->
        <h1
          class="text-3xl lg:text-4xl font-bold text-white mb-4 leading-tight"
        >
          {{ report.title }}
        </h1>

        <!-- Subtitle -->
        <p
          v-if="report.subtitle"
          class="text-lg lg:text-xl text-gray-200 mb-6 leading-relaxed"
        >
          {{ report.subtitle }}
        </p>

        <!-- Metadata -->
        <div
          class="flex flex-wrap items-center gap-6 text-sm text-gray-300 mb-6"
        >
          <div v-if="report.datePublished" class="flex items-center gap-2">
            <CalendarIcon class="w-4 h-4" />
            <span>Published {{ formatDate(report.datePublished) }}</span>
          </div>
          <div v-if="report.dateUpdated" class="flex items-center gap-2">
            <ClockIcon class="w-4 h-4" />
            <span>Updated {{ formatDate(report.dateUpdated) }}</span>
          </div>
          <div v-if="report.id" class="flex items-center gap-2">
            <HashIcon class="w-4 h-4" />
            <span>{{ report.id }}</span>
          </div>
        </div>

        <!-- CTA Button -->
        <div v-if="report.cta" class="mb-8">
          <Button
            size="lg"
            class="bg-white text-gray-900 hover:bg-gray-100 font-semibold px-8 py-3 text-base"
          >
            {{ report.cta }}
            <ArrowRightIcon class="w-4 h-4 ml-2" />
          </Button>
        </div>
      </div>
    </div>

    <!-- Report Insights Section -->
    <div class="border-t border-white/10 bg-black/20 backdrop-blur-sm">
      <div class="px-6 py-8 lg:px-8">
        <div class="max-w-4xl">
          <h3
            class="text-white font-semibold text-xl mb-4 flex items-center gap-3"
          >
            <div class="w-1 h-6 bg-blue-500 rounded-full"></div>
            Report Insights
          </h3>
          <div class="grid gap-4 md:grid-cols-2">
            <div
              class="p-4 bg-white/10 backdrop-blur-sm rounded-lg border border-white/20"
            >
              <h4 class="text-white font-medium mb-2">Key Metrics</h4>
              <p class="text-gray-200 text-sm leading-relaxed">
                Performance indicators and benchmarks for
                {{ report.theme || "your campaign" }}.
              </p>
            </div>
            <div
              class="p-4 bg-white/10 backdrop-blur-sm rounded-lg border border-white/20"
            >
              <h4 class="text-white font-medium mb-2">Actionable Insights</h4>
              <p class="text-gray-200 text-sm leading-relaxed">
                Strategic recommendations based on data analysis and trends.
              </p>
            </div>
          </div>

          <!-- Mock Chart Area -->
          <div
            class="mt-6 p-6 bg-white/5 backdrop-blur-sm rounded-xl border border-white/10"
          >
            <div class="flex items-center justify-between mb-4">
              <h4 class="text-white font-medium">Performance Overview</h4>
              <div class="flex gap-2">
                <div class="w-3 h-3 bg-blue-400 rounded-full"></div>
                <div class="w-3 h-3 bg-green-400 rounded-full"></div>
                <div class="w-3 h-3 bg-yellow-400 rounded-full"></div>
              </div>
            </div>
            <div
              class="h-32 bg-white/5 rounded-lg flex items-center justify-center"
            >
              <p class="text-gray-300 text-sm">
                Interactive charts would be displayed here
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import Button from "@/components/ui/Button.vue";
import {
  CalendarIcon,
  ClockIcon,
  HashIcon,
  ArrowRightIcon,
} from "lucide-vue-next";

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

const getDummyHeroBackground = (report: Report): string => {
  const backgrounds = {
    "Creative Benchmarking":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="800" height="400" viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="creative-hero" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#667eea;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#764ba2;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="800" height="400" fill="url(#creative-hero)"/>
        <circle cx="150" cy="100" r="80" fill="white" opacity="0.1"/>
        <circle cx="650" cy="300" r="120" fill="white" opacity="0.1"/>
        <rect x="100" y="320" width="160" height="8" fill="white" opacity="0.3"/>
        <rect x="400" y="250" width="240" height="8" fill="white" opacity="0.3"/>
        <circle cx="400" cy="180" r="40" fill="white" opacity="0.15"/>
      </svg>
    `),
    "Efficiency Trends":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="800" height="400" viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="efficiency-hero" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#11998e;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#38ef7d;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="800" height="400" fill="url(#efficiency-hero)"/>
        <path d="M100,350 Q300,280 500,250 T700,200" stroke="white" stroke-width="4" fill="none" opacity="0.4"/>
        <circle cx="300" cy="280" r="6" fill="white" opacity="0.6"/>
        <circle cx="500" cy="250" r="6" fill="white" opacity="0.6"/>
        <circle cx="700" cy="200" r="6" fill="white" opacity="0.6"/>
        <rect x="250" y="100" width="300" height="60" rx="8" fill="white" opacity="0.1"/>
      </svg>
    `),
    "Audience Segments":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="800" height="400" viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="audience-hero" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#fc4a1a;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#f7b733;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="800" height="400" fill="url(#audience-hero)"/>
        <circle cx="200" cy="150" r="50" fill="white" opacity="0.2"/>
        <circle cx="600" cy="150" r="50" fill="white" opacity="0.2"/>
        <circle cx="400" cy="250" r="60" fill="white" opacity="0.2"/>
        <circle cx="250" cy="320" r="35" fill="white" opacity="0.2"/>
        <circle cx="550" cy="320" r="35" fill="white" opacity="0.2"/>
        <rect x="150" y="50" width="500" height="40" rx="20" fill="white" opacity="0.1"/>
      </svg>
    `),
    "Predictive Insights":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="800" height="400" viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="predictive-hero" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#4568dc;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#b06ab3;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="800" height="400" fill="url(#predictive-hero)"/>
        <path d="M100,200 L200,160 L300,180 L400,120 L500,140 L600,80 L700,100" stroke="white" stroke-width="3" fill="none" opacity="0.4"/>
        <rect x="150" y="320" width="40" height="60" fill="white" opacity="0.2"/>
        <rect x="250" y="300" width="40" height="80" fill="white" opacity="0.2"/>
        <rect x="350" y="280" width="40" height="100" fill="white" opacity="0.2"/>
        <rect x="450" y="260" width="40" height="120" fill="white" opacity="0.2"/>
        <rect x="550" y="240" width="40" height="140" fill="white" opacity="0.2"/>
      </svg>
    `),
    "Creative Hook":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="800" height="400" viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="hook-hero" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#ff9a9e;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#fecfef;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="800" height="400" fill="url(#hook-hero)"/>
        <rect x="100" y="80" width="150" height="90" rx="12" fill="white" opacity="0.2"/>
        <rect x="280" y="120" width="120" height="75" rx="12" fill="white" opacity="0.2"/>
        <rect x="430" y="90" width="140" height="85" rx="12" fill="white" opacity="0.2"/>
        <rect x="200" y="220" width="400" height="120" rx="16" fill="white" opacity="0.3"/>
        <circle cx="400" cy="50" r="25" fill="white" opacity="0.4"/>
      </svg>
    `),
    "Performance Index":
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="800" height="400" viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="performance-hero" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#2196F3;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#21CBF3;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="800" height="400" fill="url(#performance-hero)"/>
        <circle cx="400" cy="200" r="120" stroke="white" stroke-width="4" fill="none" opacity="0.3"/>
        <circle cx="400" cy="200" r="80" stroke="white" stroke-width="3" fill="none" opacity="0.4"/>
        <circle cx="400" cy="200" r="40" fill="white" opacity="0.5"/>
        <rect x="100" y="100" width="200" height="8" fill="white" opacity="0.3"/>
        <rect x="500" y="100" width="200" height="8" fill="white" opacity="0.3"/>
        <rect x="100" y="300" width="200" height="8" fill="white" opacity="0.3"/>
        <rect x="500" y="300" width="200" height="8" fill="white" opacity="0.3"/>
      </svg>
    `),
    default:
      "data:image/svg+xml;base64," +
      btoa(`
      <svg width="800" height="400" viewBox="0 0 800 400" xmlns="http://www.w3.org/2000/svg">
        <defs>
          <linearGradient id="default-hero" x1="0%" y1="0%" x2="100%" y2="100%">
            <stop offset="0%" style="stop-color:#6366f1;stop-opacity:1" />
            <stop offset="100%" style="stop-color:#8b5cf6;stop-opacity:1" />
          </linearGradient>
        </defs>
        <rect width="800" height="400" fill="url(#default-hero)"/>
        <rect x="100" y="150" width="600" height="100" rx="16" fill="white" opacity="0.1"/>
        <circle cx="200" cy="200" r="40" fill="white" opacity="0.2"/>
        <rect x="280" y="180" width="320" height="12" fill="white" opacity="0.3"/>
        <rect x="280" y="200" width="240" height="12" fill="white" opacity="0.3"/>
        <rect x="280" y="220" width="380" height="12" fill="white" opacity="0.3"/>
      </svg>
    `),
  };

  return (
    backgrounds[report.theme as keyof typeof backgrounds] || backgrounds.default
  );
};

const props = defineProps<{
  report: Report;
}>();

const handleImageError = (event: Event) => {
  const img = event.target as HTMLImageElement;
  img.src = getDummyHeroBackground(props.report);
};

const formatDate = (dateString: string): string => {
  try {
    return new Date(dateString).toLocaleDateString("en-US", {
      year: "numeric",
      month: "long",
      day: "numeric",
    });
  } catch {
    return dateString;
  }
};
</script>
