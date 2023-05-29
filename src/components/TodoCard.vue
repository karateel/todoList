<template>
    <div>
      <span 
      class="text-lg"
      :class="{'line-through': props.item.done}">
        {{ item.content }}
      </span>
    </div>
<div>
  <span
    class="text-sm font-bold block"
    :class="{'line-through': props.item.done}"
  >
    {{ isChecked ? item.completeDate : props.item.date }}
  </span>
</div>
  <div class="flex items-center mr-4 mt-3 mb-2">  
    <input type="checkbox" :id="`todoItem_${item.created}`" 
    v-model="isChecked"
    @change="updateDone"
    name="A3-confirmation" value="yes" class="opacity-0 absolute h-8 w-8" />  
    <div class="bg-slate-100 border-2 rounded-md border-sky-300 w-8 h-8 
    flex flex-shrink-0 justify-center items-center mr-2 focus-within:border-purple-300">  
      <svg class="fill-current hidden w-3 h-3 text-blue-600 pointer-events-none" version="1.1" viewBox="0 0 17 12" xmlns="http://www.w3.org/2000/svg">  
        <g fill="none" fill-rule="evenodd">  
          <g transform="translate(-9 -11)" fill="rgb(34 197 94)" fill-rule="nonzero">  
            <path d="m25.576 11.414c0.56558 0.55188 0.56558 1.4439 0 1.9961l-9.404 9.176c-0.28213 0.27529-0.65247 0.41385-1.0228 0.41385-0.37034 0-0.74068-0.13855-1.0228-0.41385l-4.7019-4.588c-0.56584-0.55188-0.56584-1.4442 0-1.9961 0.56558-0.55214 1.4798-0.55214 2.0456 0l3.679 3.5899 8.3812-8.1779c0.56558-0.55214 1.4798-0.55214 2.0456 0z" />  
          </g>  
        </g>  
      </svg>  
    </div>  
    <label :for="`todoItem_${item.created}`" class="select-none">{{ isChecked ? 'Undone' : 'Done' }}</label>  
  </div>  
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { TodoItem } from '../interfaces/todoItem.interface'
import { useNow, useDateFormat } from '@vueuse/core'

const props = defineProps({
  item: {
    type: Object as () => TodoItem,
    required: true,
  },
})

const isChecked = ref(props.item.done)
function updateDone(): void{
  props.item.done = isChecked.value
  item.completeDate = useDateFormat(useNow(), 'MM-DD HH:mm')
  item.created = new Date().getTime()
}
const item = props.item

</script>