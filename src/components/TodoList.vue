<script setup lang="ts">
import type { Todo } from '@/types'
import CloseIcon from '@/icons/CloseIcon.vue'
import { ref } from 'vue'

const filter = ref<'done' | 'todo' | 'all'>('all')

const props = defineProps<{
  todos: Todo[]
}>()

defineEmits<{
  (e: 'onRemove', id: string): void
  (e: 'onToggle', id: string): void
  (e: 'onFilter', filter: 'all' | 'done' | 'todo'): void
}>()

function filterTodos() {
  switch (filter.value) {
    case 'done':
      return props.todos.filter((todo) => todo.done)
    case 'todo':
      return props.todos.filter((todo) => !todo.done)
    default:
      return props.todos
  }
}
</script>

<template>
  <div class="flex justify-end gap-3 mb-4">
    <label for="all">
      <input type="radio" id="all" v-model="filter" value="all" />
      All
    </label>

    <label for="done">
      <input type="radio" id="done" v-model="filter" value="done" />
      Done
    </label>

    <label for="todo">
      <input type="radio" id="todo" v-model="filter" value="todo" />
      To do
    </label>
  </div>

  <ul>
    <li v-for="todo in filterTodos()" :key="todo.id" class="flex items-center justify-between mb-2">
      <div>
        <label class="text-lg" :class="{ 'text-blue-500': todo.done }">
          <input
            type="checkbox"
            class="mr-1"
            :checked="todo.done"
            @change="$emit('onToggle', todo.id)"
          />
          {{ todo.text }}
        </label>
      </div>
      <button @click="$emit('onRemove', todo.id)" class="ml-2">
        <CloseIcon class="w-5 h-5" />
      </button>
    </li>
  </ul>
</template>
