<template>
  <div>
    <canvas ref="barChart"></canvas>
  </div>
</template>

<script>
import { Chart, registerables } from 'chart.js';

// Registering the chart element for Chart.js
Chart.register(...registerables);

export default {
  props: {
    chartData: {
      type: Array,
      required: true,
    },
    chartLabels: {
      type: Array,
      required: true,
    },
  },
  methods: {
    updateChart() {
      new Chart(this.$refs.barChart, {
        type: 'bar',
        data: {
          labels: this.chartLabels,
          datasets: [
            {
              label: 'Average Temperature',
              backgroundColor: 'rgba(144, 238, 144, 0.9)',
              data: this.chartData,
            },
          ],
        },
        options: {
          scales: {
            y: {
              min: 0,
            },
          },
        },
      });
    },
  },
  mounted() {
    this.updateChart();
  },
};
</script>
