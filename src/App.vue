<script setup lang="ts">
import { ref } from 'vue'
import { type Todo } from './types'
import CloseIcon from './icons/CloseIcon.vue'

const todos = ref<Todo[]>([])
const newTodo = ref('')
const filter = ref('all')

function addTodo(text: string) {
  todos.value.push({
    id: Math.random().toString(36).slice(2),
    text,
    done: false
  })
}

function removeTodo(id: string) {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}

function toggleTodo(id: string) {
  todos.value = todos.value.map((todo) => (todo.id === id ? { ...todo, done: !todo.done } : todo))
}

function submit(e: Event) {
  e.preventDefault()
  addTodo(newTodo.value)
  newTodo.value = ''
}

function filterTodos() {
  switch (filter.value) {
    case 'done':
      return todos.value.filter((todo) => todo.done)
    case 'todo':
      return todos.value.filter((todo) => !todo.done)
    default:
      return todos.value
  }
}
</script>

<template>
  <div class="max-w-xl mx-auto">
    <header class="flex justify-center my-4">
      <h1 class="text-3xl font-bold">My Todos</h1>
    </header>
    <main>
      <form @submit="submit" class="mb-4">
        <input
          type="text"
          v-model="newTodo"
          placeholder="What needs to be done?"
          class="w-full p-2 border border-gray-300 rounded mb-2 focus:outline-none shadow"
        />
        <div class="flex justify-end gap-3">
          <label for="all">
            <input type="radio" id="all" value="all" v-model="filter" />
            All
          </label>

          <label for="done">
            <input type="radio" id="done" value="done" v-model="filter" />
            Done
          </label>

          <label for="todo">
            <input type="radio" id="todo" value="todo" v-model="filter" />
            Todo
          </label>
        </div>
      </form>

      <ul>
        <li
          v-for="todo in filterTodos()"
          :key="todo.id"
          class="flex items-center justify-between mb-2"
        >
          <div>
            <label class="text-lg" :class="{ 'text-blue-500': todo.done }">
              <input
                type="checkbox"
                class="mr-1"
                :checked="todo.done"
                @change="toggleTodo(todo.id)"
              />
              {{ todo.text }}
            </label>
          </div>
          <button @click="removeTodo(todo.id)" class="ml-2">
            <CloseIcon class="w-5 h-5" />
          </button>
        </li>
      </ul>
    </main>
  </div>
</template>
