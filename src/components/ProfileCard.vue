<script setup>
defineProps({
  profile: Object
})
</script>

<template>
  <div class="bg-white rounded-xl border border-gray-100 shadow-sm p-6">
    <div class="flex items-center gap-5">
      <img
        :src="profile.avatar_url"
        :alt="profile.login"
        class="w-16 h-16 rounded-full ring-2 ring-indigo-50"
      />
      <div class="text-left min-w-0">
        <h2 class="text-lg font-semibold text-gray-800 truncate">
          {{ profile.name || profile.login }}
        </h2>
        <p class="text-xs text-indigo-400">@{{ profile.login }}</p>
        <p v-if="profile.bio" class="text-sm text-gray-500 mt-1 line-clamp-2">
          {{ profile.bio }}
        </p>
      </div>
    </div>

    <div class="grid grid-cols-3 gap-3 mt-5">
      <div class="text-center py-3 bg-gray-50/80 rounded-lg">
        <div class="text-lg font-semibold text-gray-800">{{ profile.public_repos }}</div>
        <div class="text-[11px] text-gray-400 mt-0.5">Repos</div>
      </div>
      <div class="text-center py-3 bg-gray-50/80 rounded-lg">
        <div class="text-lg font-semibold text-gray-800">{{ profile.followers.toLocaleString() }}</div>
        <div class="text-[11px] text-gray-400 mt-0.5">Followers</div>
      </div>
      <div class="text-center py-3 bg-gray-50/80 rounded-lg">
        <div class="text-lg font-semibold text-gray-800">{{ profile.following.toLocaleString() }}</div>
        <div class="text-[11px] text-gray-400 mt-0.5">Following</div>
      </div>
    </div>

    <div class="flex flex-wrap gap-x-4 gap-y-1 mt-4 text-xs text-gray-400">
      <span v-if="profile.location">{{ profile.location }}</span>
      <span v-if="profile.company">{{ profile.company }}</span>
      <span v-if="profile.blog">
        <a :href="profile.blog.startsWith('http') ? profile.blog : 'https://' + profile.blog"
           target="_blank" class="text-indigo-400 hover:text-indigo-600 transition-colors">
          {{ profile.blog }}
        </a>
      </span>
      <span v-if="profile.created_at">
        Joined {{ new Date(profile.created_at).getFullYear() }}
      </span>
    </div>
  </div>
</template>
