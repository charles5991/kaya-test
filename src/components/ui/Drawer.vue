<template>
  <DrawerRoot v-model:open="open">
    <slot name="trigger" />
    <DrawerPortal>
      <DrawerOverlay class="fixed inset-0 z-50 bg-black/80" />
      <DrawerContent
        class="fixed inset-x-0 bottom-0 z-50 mt-24 flex h-auto flex-col rounded-t-[10px] border bg-background"
      >
        <div class="mx-auto mt-4 h-2 w-[100px] rounded-full bg-muted" />
        <slot />
      </DrawerContent>
    </DrawerPortal>
  </DrawerRoot>
</template>

<script setup lang="ts">
import {
  DrawerRoot,
  DrawerPortal,
  DrawerOverlay,
  DrawerContent,
} from "vaul-vue";

interface DrawerProps {
  open?: boolean;
}

withDefaults(defineProps<DrawerProps>(), {
  open: false,
});

const open = defineModel<boolean>("open", { default: false });
</script>
