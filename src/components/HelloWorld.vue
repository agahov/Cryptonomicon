<script setup lang="ts">
import { ref } from "vue";

defineProps<{ rangeFrom: number; rangeTo: number }>();

const count = ref(0);
</script>

<template>
  <p class="mt-6 space-x-4">Recommended IDE setup:</p>
  Hello
  <p class="mt-4 space-x-4">
    <a
      href="https://vitejs.dev/guide/features.html"
      class="text-emerald-500 underline transition-all hover:text-emerald-400"
      target="_blank"
    >
      Vite Documentation
    </a>
    |
    <a
      href="https://v3.vuejs.org/"
      class="text-emerald-500 underline transition-all hover:text-emerald-400"
      target="_blank"
      >Vue 3 Documentation</a
    >
  </p>

  <button
    type="button"
    class="my-6 rounded bg-gray-300 px-4 py-2 transition-all hover:bg-gray-200"
    @click="count++"
  >
    count is: {{ count }}
  </button>
</template>

<style scoped></style>
