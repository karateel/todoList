<template>
  <button @click.prevent="$emit('update:show-completed', !showCompleted)" 
  class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg py-2 px-4 md:col-span-1 col-span-2">
    {{ showCompleted ? 'Hide Completed' : 'Show Completed' }}
  </button>
</template>

<script setup lang="ts">
const props = defineProps({
    showCompleted: Boolean,
})
</script>