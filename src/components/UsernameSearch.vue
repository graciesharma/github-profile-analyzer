<script setup>
import { ref } from 'vue'

defineProps({
  disabled: Boolean
})

const emit = defineEmits(['search'])
const username = ref('')

function handleSubmit() {
  const trimmed = username.value.trim()
  if (trimmed) {
    emit('search', trimmed)
  }
}
</script>

<template>
  <form @submit.prevent="handleSubmit" class="flex gap-2 max-w-xs mx-auto">
    <input
      v-model="username"
      type="text"
      placeholder="username"
      class="flex-1 px-3.5 py-2 text-sm bg-white border border-gray-200 rounded-lg
             placeholder:text-gray-300
             focus:outline-none focus:ring-2 focus:ring-indigo-500/20 focus:border-indigo-400
             transition-all"
    />
    <button
      type="submit"
      :disabled="disabled || !username.trim()"
      class="px-4 py-2 text-sm font-medium bg-indigo-500 text-white rounded-lg
             hover:bg-indigo-600 transition-colors cursor-pointer
             disabled:opacity-30 disabled:cursor-not-allowed"
    >
      Go
    </button>
  </form>
</template>
