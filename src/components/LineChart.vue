<script>
import { Line } from 'vue-chartjs';

export default {
  name: 'LineChart',
  extends: Line,
  props: {
    chartData: Array,
    options: Object,
    chartColor: Object,
  },
  mounted() {
    const dates = this.chartData.map((data) => data.date);
    const totals = this.chartData.map((data) => data.total);

    const {
      borderColor, pointBorderColor, pointBackgroundColor, backgroundColor,
    } = this.chartColor;

    this.renderChart({
      labels: dates,
      datasets: [{
        data: totals,
        borderColor,
        pointBorderColor,
        pointBackgroundColor,
        backgroundColor,
      }],
    }, this.options);
  },
};
</script>
