<script setup lang="ts">
import { ref } from 'vue'

const newTodo = ref('')

const emit = defineEmits<{
  (e: 'onAdd', text: string): void
}>()

function submit(e: Event) {
  e.preventDefault()
  emit('onAdd', newTodo.value)
  newTodo.value = ''
}
</script>

<template>
  <form @submit="submit" class="mb-4">
    <input
      type="text"
      v-model="newTodo"
      placeholder="What needs to be done?"
      class="w-full p-2 border border-gray-300 rounded mb-2 focus:outline-none shadow"
    />
  </form>
</template>
