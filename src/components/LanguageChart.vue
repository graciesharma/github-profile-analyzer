<script setup>
import { computed } from 'vue'
import { Doughnut } from 'vue-chartjs'
import {
  Chart as ChartJS,
  ArcElement,
  Tooltip,
  Legend
} from 'chart.js'

ChartJS.register(ArcElement, Tooltip, Legend)

const props = defineProps({
  repos: Array
})

const languageCounts = computed(() => {
  const counts = {}
  for (const repo of props.repos) {
    if (repo.language && !repo.fork) {
      counts[repo.language] = (counts[repo.language] || 0) + 1
    }
  }
  return Object.entries(counts)
    .sort((a, b) => b[1] - a[1])
    .slice(0, 6)
})

const colors = [
  '#6366f1', '#a5b4fc', '#818cf8', '#c7d2fe',
  '#4f46e5', '#e0e7ff'
]

const chartData = computed(() => ({
  labels: languageCounts.value.map(([lang]) => lang),
  datasets: [{
    data: languageCounts.value.map(([, count]) => count),
    backgroundColor: colors.slice(0, languageCounts.value.length),
    borderWidth: 0,
    hoverOffset: 2
  }]
}))

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  cutout: '65%',
  plugins: {
    legend: {
      position: 'bottom',
      labels: {
        boxWidth: 8,
        padding: 12,
        font: { size: 11, family: 'system-ui' },
        color: '#9ca3af'
      }
    }
  }
}
</script>

<template>
  <div v-if="languageCounts.length" class="bg-white rounded-xl border border-gray-100 shadow-sm p-6">
    <h3 class="text-xs font-medium tracking-widest uppercase text-gray-400 mb-4">
      Languages
    </h3>
    <div class="h-48">
      <Doughnut :data="chartData" :options="chartOptions" />
    </div>
  </div>
</template>
