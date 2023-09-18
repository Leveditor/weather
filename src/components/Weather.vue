<template>
  <div class="mt-12 mb-8 max-w-6xl mx-auto px-2">
    <div class="flex flex-wrap justify-between font-bold text-white">
      <div class="bg-black mb-4 md:w-96 w-full rounded">
        <h1 class="uppercase p-2">Weather forecast | today</h1>
      </div>
      <!-- Button to change to °F or °C-->
      <div>
        <button class="bg-blue-600 px-5 rounded py-1" @click="changeTemp">{{ weatherF ? 'Change to °F' : 'Change to °C' }}</button>
      </div>
    </div>
  
    <!-- cards -->
    <div class="flex md:justify-normal justify-center flex-wrap text-center">
      <div v-for="(temp, index) in weather" :key="index" class="mr-4 mt-4">
        <div class="bg-black text-sm text-white w-60 p-2 font-bold rounded-tr rounded-tl uppercase">
          <span>{{ temp.city }} | {{ temp.textWeather }}</span>
        </div>

        <div class="bg-gray-300 flex justify-center ">
          <img :src="temp.icon"  />
        </div>

        <div class="w-60 flex text-xl overflow-x-hidden text-white text-center uppercase border-b">
          <p v-if="weatherF" class="bg-black w-2/4 p-2 border-r">{{ Math.round(temp.temp) }} ℃</p>
          <p v-else class="bg-black w-2/4 p-2 border-r">{{ Math.round(temp.temp_f) }} °F</p>
          <p class="bg-black w-2/4 p-2">{{ temp.wind }}  km/h</p>
        </div>

        <div class="w-full flex text-xl overflow-x-hidden text-white text-center rounded-br rounded-bl">
          <p class="bg-black w-full p-5">Humidity: {{ temp.humidity }}%</p>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from 'axios';
export default {
    name: 'weatherForecast',

  data() {
      return {
          weather: null,
          weatherF: true
      }
  },

  mounted() {
    this.api();
  },

  methods: {
    async api() {
      // name of cities and location
      const cities = [
        { name: 'toronto', coordinates: '43.70,-79.42' },
        { name: 'vancouver', coordinates: '49.28,-123.13' },
        { name: 'new york', coordinates: '40.71,-74.01' },
        { name: 'são paulo', coordinates: '-23.55,-46.64' },
        { name: 'seoul', coordinates: '37.57,126.98' },
        { name: 'dubai', coordinates: '25.131412,55.189937' },
        { name: 'los angeles', coordinates: '34.043022,-118.244735' },
        { name: 'miami', coordinates: '25.781940,-80.191561' },
      ];

      const apiKey = 'e0e40b03d9msh8ad8f4839a8a85dp1eba80jsn8c269f7808ed';
      const weatherData = [];

      try {
        await Promise.all(
          cities.map(async (city) => {
            const options = {
              method: 'GET',
              url: 'https://weatherapi-com.p.rapidapi.com/current.json', // accessing the API
              params: { q: city.coordinates }, // cities location
              headers: {
                'X-RapidAPI-Key': apiKey,
                'X-RapidAPI-Host': 'weatherapi-com.p.rapidapi.com',
              },
            };

            // api data
            const response = await axios.request(options);
            const data = response.data;

            // Adding the weather data to the weatherData array
            weatherData.push({
              city: city.name,
              temp: data.current.temp_c,
              temp_f: data.current.temp_f,
              wind: data.current.wind_kph,
              humidity: data.current.humidity,
              icon: data.current.condition.icon,
              textWeather: data.current.condition.text
            });
          })
        );

        this.weather = weatherData; 
      } catch (error) {
        console.error(error);
      }
    },

    changeTemp() {
      this.weatherF = !this.weatherF;
    }
  }
}

</script>