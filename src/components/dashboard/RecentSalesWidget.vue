
<script setup>
import { monthLabels, uniqueVisitorsExclSAIndiaByMonth } from '@/data/dashboard';
import { useLayout } from '@/layout/composables/layout';
import { onMounted, ref, watch } from 'vue';

const { getPrimary, getSurface, isDarkTheme } = useLayout();

const chartData = ref(null);
const chartOptions = ref(null);

// ✅ Set Unique Visitors data
function setChartData() {
    const documentStyle = getComputedStyle(document.documentElement);

    return {
        labels: monthLabels,
        datasets: [
            {
                type: 'bar',
                label: 'Unique Visitors (Minus SA/India)',
                backgroundColor: documentStyle.getPropertyValue('--p-primary-400'),
                data: uniqueVisitorsExclSAIndiaByMonth,
                borderRadius: {
                    topLeft: 6,
                    topRight: 6
                },
                borderSkipped: false,
                barThickness: 24
            }
        ]
    };
}

// ✅ Chart styling
function setChartOptions() {
    const documentStyle = getComputedStyle(document.documentElement);
    const borderColor = documentStyle.getPropertyValue('--surface-border');
    const textMutedColor = documentStyle.getPropertyValue('--text-color-secondary');

    return {
        maintainAspectRatio: false,
        aspectRatio: 0.8,
        plugins: {
            legend: {
                labels: {
                    color: textMutedColor
                }
            },
            title: {
                display: true,
                text: 'Unique Visitors (Minus SA/India) - 2024',
                color: textMutedColor
            }
        },
        scales: {
            x: {
                ticks: { color: textMutedColor },
                grid: {
                    color: 'transparent',
                    borderColor: 'transparent'
                }
            },
            y: {
                ticks: { color: textMutedColor },
                grid: {
                    color: borderColor,
                    borderColor: 'transparent',
                    drawTicks: false
                }
            }
        }
    };
}

watch([getPrimary, getSurface, isDarkTheme], () => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});

onMounted(() => {
    chartData.value = setChartData();
    chartOptions.value = setChartOptions();
});
</script>

<template>
    <div class="card">
        <div class="font-semibold text-xl mb-4">Unique Visitors Report</div>
        <Chart type="bar" :data="chartData" :options="chartOptions" class="h-80" />
    </div>
</template>

