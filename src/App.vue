<script setup>
import { ref } from 'vue'

import UsernameSearch from './components/UsernameSearch.vue'
import ProfileCard from './components/ProfileCard.vue'
import TopRepos from './components/TopRepos.vue'
import LanguageChart from './components/LanguageChart.vue'
import ActivityChart from './components/ActivityChart.vue'
import RepoTimeline from './components/RepoTimeline.vue'

const profile = ref(null)
const repos = ref([])
const loading = ref(false)
const error = ref(null)

async function fetchUser(username) {
  profile.value = null
  repos.value = []
  error.value = null
  loading.value = true

  try {
    const [userRes, reposRes] = await Promise.all([
      fetch(`https://api.github.com/users/${username}`),
      fetch(`https://api.github.com/users/${username}/repos?per_page=100&sort=updated`)
    ])

    if (!userRes.ok) {
      if (userRes.status === 404) {
        throw new Error(`User "${username}" not found`)
      }
      throw new Error('Something went wrong — try again in a minute')
    }

    profile.value = await userRes.json()
    repos.value = reposRes.ok ? await reposRes.json() : []
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 text-gray-600">
    <!-- Minimal header -->
    <header class="pt-16 pb-10">
      <div class="max-w-xl mx-auto px-6 text-center">
        <p class="text-xs font-medium tracking-widest uppercase text-indigo-400 mb-3">
          GitHub Profile Analyzer
        </p>
        <h1 class="text-2xl font-semibold text-gray-800 mb-2">
          Explore any developer
        </h1>
        <p class="text-sm text-gray-400 mb-8">
          Public repos, languages, stars, and activity — at a glance.
        </p>
        <UsernameSearch @search="fetchUser" :disabled="loading" />
      </div>
    </header>

    <main class="max-w-xl mx-auto px-6 pb-20">
      <!-- Loading -->
      <div v-if="loading" class="text-center py-16">
        <div class="inline-block w-5 h-5 border-2 border-indigo-200 border-t-indigo-500 rounded-full animate-spin"></div>
      </div>

      <!-- Error -->
      <div v-else-if="error" class="text-center py-16">
        <p class="text-sm text-rose-400">{{ error }}</p>
      </div>

      <!-- Results -->
      <div v-else-if="profile" class="space-y-5">
        <ProfileCard :profile="profile" />

        <!-- Two charts side by side -->
        <div class="grid grid-cols-1 sm:grid-cols-2 gap-5">
          <LanguageChart :repos="repos" />
          <ActivityChart :repos="repos" />
        </div>

        <TopRepos :repos="repos" />
        <RepoTimeline :repos="repos" />
      </div>

      <!-- Empty state -->
      <div v-else class="text-center py-16">
        <p class="text-sm text-gray-300">
          Try searching for <span class="text-gray-400">torvalds</span>,
          <span class="text-gray-400">sindresorhus</span>, or
          <span class="text-gray-400">tj</span>
        </p>
      </div>
    </main>
  </div>
</template>
