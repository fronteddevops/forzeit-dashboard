<script setup>
import { trialSignupConversionRatesByMonth } from '@/data/dashboard';
import { useLayout } from '@/layout/composables/layout';
import { onMounted, ref, watch } from 'vue';

const { getPrimary, getSurface, isDarkTheme } = useLayout();
const lineData = ref(null);
const lineOptions = ref(null);

// ✅ Conversion rate JSON
const trialJson = trialSignupConversionRatesByMonth;

onMounted(() => {
  setColorOptions();
});

function setColorOptions() {
  const documentStyle = getComputedStyle(document.documentElement);
  const textColor = documentStyle.getPropertyValue('--text-color');
  const textColorSecondary = documentStyle.getPropertyValue('--text-color-secondary');
  const surfaceBorder = documentStyle.getPropertyValue('--surface-border');

  const maxVal = Math.max(...trialJson.map(i => i.convRate));
  const normalizedData = trialJson.map(i => (i.convRate / maxVal) * 100);

  const benchmarkNormalized = (9 / maxVal) * 100;

  const yMax = Math.ceil(Math.max(100, benchmarkNormalized));
lineData.value = {
  labels: trialJson.map(item => item.month),
  datasets: [
    {
      label: 'Trial Signup Conversion Rate (%)',
      data: trialJson.map(item => item.convRate), // raw %
      fill: false,
      borderColor: documentStyle.getPropertyValue('--p-primary-500'),
      backgroundColor: documentStyle.getPropertyValue('--p-primary-500'),
      tension: 0.3,
      yAxisID: 'y'
    },
    {
      label: 'Benchmark (9%)',
      data: new Array(trialJson.length).fill(9), // ✅ fixed 9%
      borderColor: 'red',
      backgroundColor: 'red',
      borderWidth: 1,
      borderDash: [, 0],
      pointRadius: 0,
      fill: false,
      tension: 0,
      yAxisID: 'y'
    }
  ]
};

// ✅ Ensure Y max covers 9%
lineOptions.value = {
  responsive: true,
  maintainAspectRatio: false,
  layout: {
    padding: { top: 16, right: 24, bottom: 24, left: 12 }
  },
  plugins: {
    legend: { labels: { color: textColor } }
  },
  scales: {
    y: {
      beginAtZero: true,
      max: 10, // covers up to 9%
      ticks: {
        color: textColorSecondary,
        callback: (value) => value + "%"
      },
      grid: { color: surfaceBorder, drawBorder: false }
    }
  }
};

}

watch([getPrimary, getSurface, isDarkTheme], () => {
  setColorOptions();
}, { immediate: true });
</script>

<template>
  <div class="card">
    <div class="font-semibold text-xl mb-4">Trial Signup Conversion Rate</div>
    <Chart type="line" :data="lineData" :options="lineOptions" style="height: 360px"></Chart>
  </div>
</template>
