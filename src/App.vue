<template>
  <div id="app" class="container">
    <div class="row mt-5">
      <div class="col text-center">
        <h1>COVID-19 Data Visualization</h1>
      </div>
    </div>

    <div class="row mt-5">
      <div class="col text-center">
        <div class="btn-group" role="group">
          <button
            type="button"
            class="btn"
            :class="{active: active === 'confirmed'}"
            @click="active = 'confirmed'"
          >
            Confirmed
          </button>
          <button
            type="button"
            class="btn"
            :class="{active: active === 'deceased'}"
            @click="active = 'deceased'"
          >
            Deaths
          </button>
          <button
            type="button"
            class="btn"
            :class="{active: active === 'recovered'}"
            @click="active = 'recovered'"
          >
            Recovered
          </button>
        </div>
      </div>
    </div>

    <div v-if="confirmedCases.length > 0 && active === 'confirmed'" class="row mt-5">
      <div class="col">
        <LineChart
          :chartData="confirmedCases"
          :options="chartOptions"
          :chartColor="confirmedChartColors"
          key='confirmed'
        />
      </div>
    </div>

    <div v-if="deceasedCases.length > 0 && active === 'deceased'" class="row mt-5">
      <div class="col">
        <LineChart
          :chartData="deceasedCases"
          :options="chartOptions"
          :chartColor="deceasedChartColors"
          key='deceased'
        />
      </div>
    </div>

    <div v-if="recoveredCases.length > 0 && active === 'recovered'" class="row mt-5">
      <div class="col">
        <LineChart
          :chartData="recoveredCases"
          :options="chartOptions"
          :chartColor="recoveredChartColors"
          key='recovered'
        />
      </div>
    </div>
  </div>
</template>

<script>
import ky from 'ky';
import moment from 'moment';

import LineChart from '@/components/LineChart.vue';

export default {
  name: 'App',
  components: {
    LineChart,
  },

  data() {
    return {
      active: 'confirmed',
      confirmedCases: [],
      deceasedCases: [],
      recoveredCases: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false,
        legend: {
          display: false,
        },
        scales: {
          xAxes: [{
            gridLines: {
              display: true,
              color: 'rgba(238,238,238, 0.3)',
              zeroLineColor: '#eeeeee',
            },
            ticks: {
              fontColor: '#eeeeee',
            },
          }],
          yAxes: [{
            gridLines: {
              display: true,
              color: 'rgba(238,238,238, 0.3)',
              zeroLineColor: '#eeeeee',
            },
            ticks: {
              fontColor: '#eeeeee',
            },
          }],
        },
      },
      confirmedChartColors: {
        borderColor: '#fcd570',
        pointBorderColor: '#ffa600',
        pointBackgroundColor: '#ffefab',
        backgroundColor: '#ffefab',
      },
      deceasedChartColors: {
        borderColor: '#ffb0a0',
        pointBorderColor: '#ff6361',
        pointBackgroundColor: '#ffddd4',
        backgroundColor: '#ffddd4',
      },
      recoveredChartColors: {
        borderColor: '#a6f8bf',
        pointBorderColor: '#31e981',
        pointBackgroundColor: '#ddffe7',
        backgroundColor: '#ddffe7',
      },
    };
  },

  async created() {
    try {
      const data = await ky.get('https://api.covid19api.com/country/brazil').json();

      data.forEach((element) => {
        const date = moment(element.Date).format('DD/MM');

        this.confirmedCases.push({ date, total: element.Confirmed });
        this.deceasedCases.push({ date, total: element.Deaths });
        this.recoveredCases.push({ date, total: element.Recovered });
      });
    } catch (error) {
      console.log(error);
    }
  },

  methods: {
    setActive(type) {
      this.active = type;
    },
  },
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Rubik&display=swap');

body {
  background-color: #222831 !important;
  color: #eeeeee !important;
  font-family: 'Rubik', sans-serif !important;
}

button {
  background-color: #393e46 !important;
  color: #eeeeee !important;
  border: solid 1px #eeeeee !important;
}

.active {
  background-color: #eeeeee !important;
  color: #393e46 !important;
}
</style>
