<template>
  <div class="flex justify-center items-center gap-4 mt-6">
    <button class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg py-2 px-4 md:col-span-1 disabled:opacity-60" @click="prevPage" :disabled="currentPage === 1">Previous</button>
    <span>{{ currentPage }} / {{ totalPages }}</span>
    <button class="bg-blue-500 hover:bg-blue-600 text-white rounded-lg py-2 px-4 md:col-span-1 disabled:opacity-60" @click="nextPage" :disabled="currentPage === totalPages">Next</button>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  itemsPerPage: number;
  totalItems: number;
  currentPage: number;
  updateCurrentPage: (page: number) => void;
}

const props = defineProps<Props>();

const totalPages = computed(() => Math.ceil(props.totalItems / props.itemsPerPage));

function nextPage() {
  if (props.currentPage < totalPages.value) {
    props.updateCurrentPage(props.currentPage + 1);
  }
}

function prevPage() {
  if (props.currentPage > 1) {
    props.updateCurrentPage(props.currentPage - 1);
  }
}
</script>