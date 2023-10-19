<template>
  <div class="form">
    <h1>Weather Data</h1>
    <form @submit.prevent="submitForm">
      <div class="form-group">
        <label for="city">City: </label>
        <input type="text" id="city" class="form-control" v-model="formData.city" required>
      </div>

      <div class="form-group">
        <label for="startdate">Start Date: </label>
        <input type="date" id="startdate" class="form-control" v-model="formData.startdate" required>
      </div>

      <div class="form-group">
        <label for="enddate">End Date: </label>
        <input type="date" id="enddate" class="form-control" v-model="formData.enddate" required>
      </div>

      <button type="submit" class="btn btn-danger mt-3">Submit</button>
    </form>
    <div v-if="chartdata.temperatures.length">
      <bar-chart :chart-data="chartdata.temperatures" :chart-labels="chartdata.chartdays"></bar-chart>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import BarChart from '../components/BarChart.vue';

export default {
  components: {
    BarChart,
  },
  data() {
    return {
      formData: {
        city: '',
        startdate: '',
        enddate: '',
      },
      chartdata: {
        temperatures: [],
        chartdays: [],
      },
    };
  },
  methods: {
    async submitForm() {
      const options = {
        method: 'GET',
        url: 'https://weatherapi-com.p.rapidapi.com/history.json',
        params: {
          q: this.formData.city,
          dt: this.formData.startdate,
          lang: 'en',
          end_dt: this.formData.enddate,
        },
        headers: {
          'X-RapidAPI-Key': '8445e6f8admsh7610eefac5796f8p1db4a1jsn547d175b7b44',
          'X-RapidAPI-Host': 'weatherapi-com.p.rapidapi.com',
        },
      };

      try {
        const response = await axios.request(options);
        const temperatures = response.data.forecast.forecastday.map((day) => day.day.avgtemp_f);
        const chartdays = response.data.forecast.forecastday.map((day) => day.date);
        this.chartdata = { temperatures, chartdays };
      } catch (error) {
        console.error(error);
      }
    },
  },
};
</script>

<style>
.form {
  padding: 8rem;
}
</style>