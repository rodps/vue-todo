<script setup lang="ts">
import { ref } from 'vue'
import { type Todo } from '@/types'
import FormAddTodo from '@/components/FormAddTodo.vue'
import TodoList from './components/TodoList.vue'

const todos = ref<Todo[]>([])

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
</script>

<template>
  <div class="max-w-xl mx-auto">
    <header class="flex justify-center my-4">
      <h1 class="text-3xl font-bold">My Todos</h1>
    </header>
    <main>
      <FormAddTodo @on-add="addTodo" />
      <TodoList :todos="todos" @on-remove="removeTodo" @on-toggle="toggleTodo" />
    </main>
  </div>
</template>
