<template>
  <div class="xs:pt-4">
    <form @submit.prevent="saveTodo" class="xs:rounded-lg bg-slate-800 shadow-lg p-6">
      <input type="text" 
      class="border border-gragy-400 rounded-lg py-2 px-3 w-full focus:border-purple-500"
      placeholder="E.g. Go to vet" v-model="todoContent">
      <div class="grid grid-cols-2 md:grid-cols-4 gap-3 mt-4">
        <button class="bg-indigo-500 hover:bg-indigo-600 text-white rounded-lg py-2 px-4 md:col-span-1">Save ToDo</button>
        <input type="text" placeholder="Search" name="todoSearch" id="todoSearch" 
        class="border border-gray-400 rounded-lg py-2 px-3 md:col-span-2 focus:border-purple-500" v-model="todoSearch">
        <CompletedButton :show-completed="showCompleted" @update:show-completed="(value) => { showCompleted = value }" />
      </div>
    </form>
    <TodoList :todo-item="filteredTodoItems" v-if="filteredTodoItems.length" />
    <NoTodos v-else>
      No ToDos here
    </NoTodos>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted, computed } from 'vue'
import TodoList from './TodoList.vue';
import CompletedButton from './CompletedButton.vue';
import { TodoItem } from '../interfaces/todoItem.interface'
import { useNow, useDateFormat } from '@vueuse/core'
import NoTodos from './NoTodos.vue';


const todoContent = ref('')
const todoItem = ref<TodoItem[]>([])
const todoSearch = ref('')
const showCompleted = ref<Boolean>()

defineEmits(["updateTodo"]);

function saveTodo() {
  if(todoContent.value.trim() === ''){
    return
  }
  const newTodo: TodoItem = {
    content: todoContent.value,
    done: false,
    date: useDateFormat(useNow(), 'MM-DD HH:mm'),
    created: new Date().getTime()
  }
  todoItem.value.push(newTodo)
  todoContent.value = ''
}

watch(todoItem, (newTodoItem) => {
  localStorage.setItem('todoItem', JSON.stringify(newTodoItem))
}, { deep: true })

watch(todoContent, (newTodoContent) => {
  localStorage.setItem('todoContent', newTodoContent)
})

watch(showCompleted, (showCompletedVal) => {
    localStorage.setItem('showCompleted', JSON.stringify(showCompletedVal))
})

onMounted(() => {
  const storedTodoItem = JSON.parse(localStorage.getItem('todoItem') || '[]')
  todoItem.value = storedTodoItem
  showCompleted.value = JSON.parse(localStorage.getItem('showCompleted') || 'false')
})

const filteredTodoItems = computed(() => {
  if (!showCompleted.value) {
    return todoItem.value.filter((item: {
      content: string,
      done: boolean
    }) => {
      return item.content.toLowerCase().includes(todoSearch.value.trim().toLocaleLowerCase())
    })
  } else {
    return todoItem.value.filter((item: {
      content: string,
      done: boolean
    }) => {
      return item.done && item.content.toLowerCase().includes(todoSearch.value.trim().toLocaleLowerCase())
    })
  }
})
</script>