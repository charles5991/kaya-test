<template>
  <DrawerRoot v-model:open="open" :direction="props.direction">
    <slot name="trigger" />
    <DrawerPortal>
      <DrawerOverlay class="fixed inset-0 z-50 bg-black/60 backdrop-blur-sm" />
      <DrawerContent
        :class="cn(sheetVariants({ side: props.side }), props.className)"
      >
        <!-- Floating Close Button -->
        <button
          @click="() => (open = false)"
          class="absolute top-4 right-4 z-20 w-8 h-8 rounded-full bg-black/20 hover:bg-black/30 backdrop-blur-sm flex items-center justify-center transition-all duration-200 group border border-white/20"
        >
          <svg
            class="w-4 h-4 text-white group-hover:text-white/90 transition-colors"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M6 18L18 6M6 6l12 12"
            />
          </svg>
        </button>

        <!-- Sheet Content -->
        <div class="h-full overflow-y-auto">
          <slot />
        </div>
      </DrawerContent>
    </DrawerPortal>
  </DrawerRoot>
</template>

<script setup lang="ts">
import { cva } from "class-variance-authority";
import {
  DrawerRoot,
  DrawerPortal,
  DrawerOverlay,
  DrawerContent,
} from "vaul-vue";
import { cn } from "@/lib/utils";

const sheetVariants = cva(
  "fixed z-50 flex flex-col bg-background shadow-2xl transition-all ease-in-out data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:duration-300 data-[state=open]:duration-500",
  {
    variants: {
      side: {
        top: "inset-x-0 top-0 border-b border-border/20 data-[state=closed]:slide-out-to-top data-[state=open]:slide-in-from-top",
        bottom:
          "inset-x-0 bottom-0 border-t border-border/20 max-h-[85vh] rounded-t-xl data-[state=closed]:slide-out-to-bottom data-[state=open]:slide-in-from-bottom",
        left: "inset-y-0 left-0 h-full w-3/4 border-r border-border/20 data-[state=closed]:slide-out-to-left data-[state=open]:slide-in-from-left sm:max-w-sm",
        right:
          "inset-y-0 right-0 h-full w-full sm:w-3/4 lg:w-2/3 xl:w-1/2 max-w-4xl border-l border-border/20 data-[state=closed]:slide-out-to-right data-[state=open]:slide-in-from-right",
      },
    },
    defaultVariants: {
      side: "right",
    },
  }
);

interface SheetProps {
  side?: "top" | "bottom" | "left" | "right";
  open?: boolean;
  className?: string;
  direction?: "top" | "bottom" | "left" | "right";
}

const props = withDefaults(defineProps<SheetProps>(), {
  open: false,
  side: "right",
  direction: "right",
});

const open = defineModel<boolean>("open", { default: false });
</script>
