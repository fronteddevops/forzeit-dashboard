<script setup>
import { revenueMetricsByMonth as revenueJson } from '@/data/dashboard';
import { useLayout } from '@/layout/composables/layout';
import { onMounted, ref, watch } from 'vue';

const { getPrimary, getSurface, isDarkTheme } = useLayout();

const mixedData = ref(null);
const mixedOptions = ref(null);

// data imported from centralized source

// ✅ Create chart data (all bar datasets)
function setMixedData() {
  return {
    labels: revenueJson.map(item => item.label),
    datasets: [
      {
        label: "New MRR (£)",
        type: "bar",
        backgroundColor: "#42A5F5",
        data: revenueJson.map(item => item.newMRR),
        borderRadius: 6,
        barThickness: 20
      },
      {
        label: "Total MRR (£)",
        type: "bar",
        backgroundColor: "#EC407A",
        data: revenueJson.map(item => item.totalMRR),
        borderRadius: 6,
        barThickness: 20
      },
      {
        label: "Total ARR (£)",
        type: "bar",
        backgroundColor: "#26C6DA",
        data: revenueJson.map(item => item.totalARR),
        borderRadius: 6,
        barThickness: 20
      }
    ]
  };
}

// ✅ Chart options (only 1 Y-axis, grouped bars)
function setMixedOptions() {
  const documentStyle = getComputedStyle(document.documentElement);
  const borderColor = documentStyle.getPropertyValue('--surface-border');
  const textMutedColor = documentStyle.getPropertyValue('--text-color-secondary');

  return {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      title: {
        display: true,
        text: "Revenue Metrics - MRR & ARR",
        color: textMutedColor
      },
      legend: {
        labels: { color: textMutedColor }
      }
    },
    scales: {
      x: {
        stacked: false, // grouped bars
        ticks: { color: textMutedColor },
        grid: { display: false }
      },
      y: {
        beginAtZero: true,
        stacked: false,
        ticks: {
          color: textMutedColor,
          callback: val => "£" + val.toLocaleString()
        },
        grid: { color: borderColor }
      }
    }
  };
}

// ✅ Watch for theme changes
watch([getPrimary, getSurface, isDarkTheme], () => {
  mixedData.value = setMixedData();
  mixedOptions.value = setMixedOptions();
});

// ✅ Initial render
onMounted(() => {
  mixedData.value = setMixedData();
  mixedOptions.value = setMixedOptions();
});
</script>

<template>
  <div class="card">
    <div class="font-semibold text-xl mb-4">Revenue (MRR & ARR)</div>
    <Chart type="bar" :data="mixedData" :options="mixedOptions" class="h-96" />
  </div>
</template>
