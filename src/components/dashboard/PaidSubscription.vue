<script setup>
import { monthLabels as labels, paidConversionRatesByMonth as paidConvRates } from '@/data/dashboard';
import { useLayout } from '@/layout/composables/layout';
import { onMounted, ref, watch } from 'vue';

const { getPrimary, getSurface, isDarkTheme } = useLayout();

const paidSubsData = ref(null);
const paidSubsOptions = ref(null);
const paidConvData = ref(null);
const paidConvOptions = ref(null);



function setPaidConvChart() {
  const documentStyle = getComputedStyle(document.documentElement);
  return {
    labels,
    datasets: [
      {
        type: 'bar',
        label: 'Paid Sub Conv. Rate (%)',
        borderRadius: {
                    topLeft: 6,
                    topRight: 6
                },
        backgroundColor: documentStyle.getPropertyValue('--p-primary-400'),
        data: paidConvRates
      },
      {
        type: 'line',
        label: 'Benchmark (20%)',
        borderColor: 'red',
        backgroundColor: 'red',
        borderWidth: 1,
        borderDash: [, 0],  
        pointRadius: 0,
        tension: 0,
        fill: false,
        data: Array(labels.length).fill(20) // straight 20% line
      }
    ]
  };
}

function setPaidConvOptions() {
  const documentStyle = getComputedStyle(document.documentElement);
  const borderColor = documentStyle.getPropertyValue('--surface-border');
  const textMutedColor = documentStyle.getPropertyValue('--text-color-secondary');

  return {
    maintainAspectRatio: false,
    plugins: {
      title: {
        display: true,
        text: 'Paid Subscription Conversion Rate - 2024',
        color: textMutedColor
      }
    },
    scales: {
      x: {
        ticks: { color: textMutedColor },
        grid: { color: 'transparent', borderColor: 'transparent' }
      },
      y: {
        beginAtZero: true,
        max: 25, // ensure 20% benchmark fits
        ticks: {
          color: textMutedColor,
          callback: (val) => val + '%'
        },
        grid: { color: borderColor, borderColor: 'transparent', drawTicks: false }
      }
    }
  };
}

watch([getPrimary, getSurface, isDarkTheme], () => {

  paidConvData.value = setPaidConvChart();
  paidConvOptions.value = setPaidConvOptions();
});

onMounted(() => {

  paidConvData.value = setPaidConvChart();
  paidConvOptions.value = setPaidConvOptions();
});
</script>

<template>
  <div class="grid grid-cols-1 gap-6">

    <!-- Paid Conversion Rate Bar + Benchmark Line -->
    <div class="card">
      <div class="font-semibold text-xl mb-4">Paid Subscription Conversion Rate</div>
      <Chart type="bar" :data="paidConvData" :options="paidConvOptions" class="h-80" />
    </div>
  </div>
</template>
