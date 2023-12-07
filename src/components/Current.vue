<template>
    <div class="weather-container mb-10 wi-wom-10 p-10">
      <h1 class="text-3xl font-semibold mb-4 bg-gradient-to-t">Current Weather</h1>
      
      <div v-if="loading" class="text-gray-600">Loading...</div>
  
      <div v-else-if="weatherData">
        <h2 class="text-xl font-semibold mb-2 bg-gradient-to-t">{{ weatherData.name }}, {{ weatherData.sys.country }}</h2>
        <div class="flex items-left">
          <img :src="getWeatherIconUrl(weatherData.weather[0].icon)" :alt="weatherData.weather[0].description" class="mr-2">
          <p class="text-lg">{{ weatherData.weather[0].description }}</p>
        </div>
        <p class="text-4xl font-bold my-2">{{ Math.round(weatherData.main.temp) }}°C</p>
        <p>Humidity: {{ weatherData.main.humidity }}%</p>
        <p>Wind: {{ weatherData.wind.speed }} m/s</p>
      </div>
    
      <div v-else class="text-red-500">Failed to fetch weather data.</div>
    </div>
  
  </template>
  
  <script>
  export default {
    data() {
      return {
        loading: true,
        weatherData: null,
      };
    },
    mounted() {
      this.fetchWeatherData();
    },
    methods: {
      async fetchWeatherData() {
        try {
          // Get current location
          const position = await this.getCurrentLocation();
          const { latitude, longitude } = position.coords;
          console.log(latitude);
          console.log(longitude);
          // Fetch weather data
          const response = await fetch(
            `https://api.openweathermap.org/data/2.5/weather?lat=${latitude}&lon=${longitude}&units=metric&appid=4c75984395347a6bff5c69597128c2bf`
          );
          const data = await response.json();
          console.log(data)
          this.weatherData = data;
          this.loading = false;
        } catch (error) {
          console.error('Error fetching weather data:', error);
          this.loading = false;
        }
      },
      getCurrentLocation() {
        return new Promise((resolve, reject) => {
          navigator.geolocation.getCurrentPosition(resolve, reject);
        });
      },
      getWeatherIconUrl(iconCode) {
        return `http://openweathermap.org/img/w/${iconCode}.png`;
      },
    },
  };
  </script>
  
  <style scoped>
  .weather-container {
    background-color: rgb(121, 121, 237);
    opacity: 60%;
    max-width: 400px;
    margin: auto;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 8px;
    box-shadow: 0 0 10px blue;
    text-align: center;
  }
  
  </style>
  