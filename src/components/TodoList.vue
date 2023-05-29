<template>
  <div class="flex flex-col items-center bg-gray-100 rounded-lg">
    <div v-if="paginatedTodos.length" class="p-6 w-full">
      <ul class="grid grid-flow-row gap-5 
      text-neutral-600 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4">
        <li 
        class="p-4 rounded shadow-lg 
        shadow-slate-400 duration-300 
        hover:-translate-y-1"
        v-for="item in paginatedTodos" :key="item.created">
          <TodoCard :item="item" />
        </li>
      </ul>
      <TodoPagination
      v-if="sortTodos.length > 6"
      :itemsPerPage="perPage"
      :totalItems="sortTodos.length"
      :currentPage="currentPage"
      :updateCurrentPage="(page: number) => currentPage = page"
    />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed, watch, onMounted } from 'vue';
import TodoCard from './TodoCard.vue';
import TodoPagination from './TodoPagination.vue';
import  { TodoItem } from '../interfaces/todoItem.interface';

const props = defineProps({
  todoItem: {
    type: Array as () => TodoItem[],
    required: true,
    default: () => [],
  }
});

function filterIncompleteTodos(todoItem){
  return todoItem.filter((item) => !item.done);
}

function filterCompletedTodos(todoItem) {
  return todoItem.filter((item) => item.done);
}

function sortTodosDesc(todoItem) {
  return todoItem.sort((a, b) => b.created - a.created);
}

const sortTodos = computed(() => {
  const incompleteTodos = filterIncompleteTodos(props.todoItem);
  const completedTodos = filterCompletedTodos(props.todoItem);

  return [...sortTodosDesc(incompleteTodos), ...sortTodosDesc(completedTodos)];
});

const currentPage = ref(1);
const perPage = 6;

watch(currentPage, (currentPageState) => {
  localStorage.setItem('currentPage', JSON.stringify(currentPageState))
})

const paginatedTodos = computed(() => {
  const start = (currentPage.value - 1) * perPage;
  const end = start + perPage;
  return sortTodos.value.slice(start, end);
});

onMounted(() => {
  const savedPage = Number(localStorage.getItem('currentPage') || '1')
  currentPage.value = savedPage
})
</script>