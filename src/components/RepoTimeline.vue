<script setup>
import { computed } from 'vue'
import { formatDistanceToNow } from 'date-fns'

const props = defineProps({
  repos: Array
})

const recentRepos = computed(() => {
  return [...props.repos]
    .filter(r => !r.fork)
    .sort((a, b) => new Date(b.updated_at) - new Date(a.updated_at))
    .slice(0, 5)
})

function timeAgo(dateStr) {
  return formatDistanceToNow(new Date(dateStr), { addSuffix: true })
}
</script>

<template>
  <div v-if="recentRepos.length" class="bg-white rounded-xl border border-gray-100 shadow-sm p-6">
    <h3 class="text-xs font-medium tracking-widest uppercase text-gray-400 mb-4">
      Recently Active
    </h3>
    <div class="divide-y divide-gray-50">
      <div
        v-for="repo in recentRepos"
        :key="repo.id"
        class="flex items-center justify-between py-3 first:pt-0 last:pb-0"
      >
        <a
          :href="repo.html_url"
          target="_blank"
          class="text-sm text-gray-600 hover:text-indigo-500 transition-colors"
        >
          {{ repo.name }}
        </a>
        <span class="text-[11px] text-gray-300">
          {{ timeAgo(repo.updated_at) }}
        </span>
      </div>
    </div>
  </div>
</template>
