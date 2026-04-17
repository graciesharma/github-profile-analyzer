<script setup>
import { computed } from 'vue'

const props = defineProps({
  repos: Array
})

const topRepos = computed(() => {
  return [...props.repos]
    .filter(r => !r.fork)
    .sort((a, b) => b.stargazers_count - a.stargazers_count)
    .slice(0, 5)
})
</script>

<template>
  <div v-if="topRepos.length" class="bg-white rounded-xl border border-gray-100 shadow-sm p-6">
    <h3 class="text-xs font-medium tracking-widest uppercase text-gray-400 mb-4">
      Top Repositories
    </h3>
    <div class="divide-y divide-gray-50">
      <div
        v-for="repo in topRepos"
        :key="repo.id"
        class="flex items-center justify-between py-3 first:pt-0 last:pb-0"
      >
        <div class="min-w-0 flex-1">
          <a
            :href="repo.html_url"
            target="_blank"
            class="text-sm font-medium text-gray-700 hover:text-indigo-500 transition-colors"
          >
            {{ repo.name }}
          </a>
          <p v-if="repo.description" class="text-xs text-gray-400 truncate mt-0.5">
            {{ repo.description }}
          </p>
        </div>
        <div class="flex items-center gap-3 ml-4 text-xs text-gray-400 shrink-0">
          <span v-if="repo.language" class="text-indigo-400/70">{{ repo.language }}</span>
          <span class="tabular-nums">{{ repo.stargazers_count }} stars</span>
        </div>
      </div>
    </div>
  </div>
</template>
