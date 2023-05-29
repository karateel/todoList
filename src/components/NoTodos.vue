<template>
  <div class="flex flex-col items-center justify-center h-screen bg-gray-100">
    <svg xmlns="http://www.w3.org/2000/svg" width="64" height="64" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-gray-400 stroke-current mb-4">
      <circle cx="12" cy="12" r="10"></circle>
      <line x1="8" y1="12" x2="16" y2="12"></line>
    </svg>
    <p class="text-gray-600 text-xl font-medium mb-2">
        <slot></slot>
    </p>
  </div>
</template>

<script setup lang="ts">
</script>