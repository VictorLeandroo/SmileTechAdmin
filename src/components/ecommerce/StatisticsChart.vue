<template>
  <div
    class="rounded-2xl border border-gray-200 bg-white px-5 pb-5 pt-5 dark:border-gray-800 dark:bg-white/[0.03] sm:px-6 sm:pt-6"
  >
    <div class="flex flex-col gap-5 mb-6 sm:flex-row sm:justify-between">
      <div class="w-full">
        <h3 class="text-lg font-semibold text-gray-800 dark:text-white/90">Estatísticas</h3>
        <p class="mt-1 text-gray-500 text-theme-sm dark:text-gray-400">
          Acompanhamento da clínica
        </p>
      </div>

      <div class="relative">
        <div class="inline-flex items-center gap-0.5 rounded-lg bg-gray-100 p-0.5 dark:bg-gray-900">
          <button
            v-for="option in options"
            :key="option.value"
            @click="selected = option.value"
            :class="[
              selected === option.value
                ? 'shadow-theme-xs text-gray-900 dark:text-white bg-white dark:bg-gray-800'
                : 'text-gray-500 dark:text-gray-400',
              'px-3 py-2 font-medium rounded-md text-theme-sm hover:text-gray-900 hover:shadow-theme-xs dark:hover:bg-gray-800 dark:hover:text-white',
            ]"
          >
            {{ option.label }}
          </button>
        </div>
      </div>
    </div>

    <div class="max-w-full overflow-x-auto custom-scrollbar">
      <div id="chartThree" class="-ml-4 min-w-[1000px] xl:min-w-full pl-2">
        <VueApexCharts type="area" height="310" :options="chartOptions" :series="series" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import VueApexCharts from 'vue3-apexcharts'

const options = [
  { value: 'optionOne', label: 'Mensal' },
  { value: 'optionTwo', label: 'Trimestral' },
  { value: 'optionThree', label: 'Anual' },
]

const selected = ref('optionOne')

const series = ref([
  {
    name: 'Consultas Realizadas',
    data: [120, 150, 140, 160, 175, 190, 200, 210, 230, 220, 250, 260],
  },
  {
    name: 'Faturamento (R$)',
    data: [30000, 32000, 31000, 35000, 37000, 42000, 45000, 47000, 50000, 51000, 56000, 59000],
  },
])

const chartOptions = ref({
  legend: {
    show: true,
    position: 'top',
    horizontalAlign: 'left',
  },
  colors: ['#0EA5E9', '#22C55E'],
  chart: {
    fontFamily: 'Outfit, sans-serif',
    type: 'area',
    toolbar: {
      show: false,
    },
  },
  fill: {
    gradient: {
      enabled: true,
      opacityFrom: 0.55,
      opacityTo: 0,
    },
  },
  stroke: {
    curve: 'straight',
    width: [2, 2],
  },
  markers: {
    size: 0,
  },
  labels: {
    show: false,
  },
  grid: {
    xaxis: { lines: { show: false } },
    yaxis: { lines: { show: true } },
  },
  dataLabels: {
    enabled: false,
  },
  tooltip: {
    y: {
      formatter: val => val.toLocaleString('pt-BR'),
    },
  },
  xaxis: {
    type: 'category',
    categories: [
      'Jan', 'Fev', 'Mar', 'Abr', 'Mai', 'Jun',
      'Jul', 'Ago', 'Set', 'Out', 'Nov', 'Dez',
    ],
    axisBorder: { show: false },
    axisTicks: { show: false },
    tooltip: { enabled: false },
  },
  yaxis: {
    title: { style: { fontSize: '0px' } },
  },
})
</script>

<style scoped>
.area-chart {
  width: 100%;
}
</style>
