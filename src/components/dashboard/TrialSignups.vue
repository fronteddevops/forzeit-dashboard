<script setup>
import { trialSignupsByMonth } from '@/data/dashboard';
import { useLayout } from '@/layout/composables/layout';
import { onMounted, ref, watch } from 'vue';

const { getPrimary, getSurface, isDarkTheme } = useLayout();
const lineData = ref(null);
const lineOptions = ref(null);
const trialJson = trialSignupsByMonth;

onMounted(() => {
  setColorOptions();
});

function setColorOptions() {
  const documentStyle = getComputedStyle(document.documentElement);
  const textColor = documentStyle.getPropertyValue('--text-color');
  const textColorSecondary = documentStyle.getPropertyValue('--text-color-secondary');
  const surfaceBorder = documentStyle.getPropertyValue('--surface-border');

  lineData.value = {
    labels: trialJson.map(item => item.month),   
    datasets: [
      {
        label: 'Trial Signups',
        data: trialJson.map(item => item.signups), 
        fill: false,
        backgroundColor: documentStyle.getPropertyValue('--p-teal-500'),
        borderColor: documentStyle.getPropertyValue('--p-teal-500'),
        tension: 0.4
      }
    ]
  };

  lineOptions.value = {
    layout: {
      padding: { top: 16, right: 24, bottom: 24, left: 12 }
    },
    plugins: {
      legend: {
        labels: {
          color: textColor
        }
      }
    },
    responsive: true,
    maintainAspectRatio: false,
    scales: {
      x: {
        ticks: { color: textColorSecondary },
        grid: { color: surfaceBorder, drawBorder: false }
      },
      y: {
        ticks: { color: textColorSecondary },
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
      <div class="font-semibold text-xl mb-4">Trial Signups</div>
      <Chart type="line" :data="lineData" :options="lineOptions" style="height: 280px"></Chart>
  </div>
</template>
