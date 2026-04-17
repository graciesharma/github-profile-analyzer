<script setup>
import { computed } from 'vue'
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  BarElement,
  CategoryScale,
  LinearScale,
  Tooltip
} from 'chart.js'

ChartJS.register(BarElement, CategoryScale, LinearScale, Tooltip)

const props = defineProps({
  repos: Array
})

const reposByYear = computed(() => {
  const years = {}
  for (const repo of props.repos) {
    if (repo.fork) continue
    const year = new Date(repo.created_at).getFullYear()
    years[year] = (years[year] || 0) + 1
  }
  return Object.entries(years).sort((a, b) => a[0] - b[0])
})

const chartData = computed(() => ({
  labels: reposByYear.value.map(([year]) => year),
  datasets: [{
    label: 'Repos',
    data: reposByYear.value.map(([, count]) => count),
    backgroundColor: '#c7d2fe',
    hoverBackgroundColor: '#6366f1',
    borderRadius: 3,
    barThickness: 14
  }]
}))

const chartOptions = {
  responsive: true,
  maintainAspectRatio: false,
  plugins: {
    legend: { display: false }
  },
  scales: {
    y: {
      beginAtZero: true,
      ticks: { stepSize: 1, font: { size: 10 }, color: '#d1d5db' },
      grid: { color: '#f3f4f6' },
      border: { display: false }
    },
    x: {
      ticks: { font: { size: 10 }, color: '#d1d5db' },
      grid: { display: false },
      border: { display: false }
    }
  }
}
</script>

<template>
  <div v-if="reposByYear.length" class="bg-white rounded-xl border border-gray-100 shadow-sm p-6">
    <h3 class="text-xs font-medium tracking-widest uppercase text-gray-400 mb-4">
      Activity
    </h3>
    <div class="h-48">
      <Bar :data="chartData" :options="chartOptions" />
    </div>
  </div>
</template>
