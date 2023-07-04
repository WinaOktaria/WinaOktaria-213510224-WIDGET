<template>
  <div class="weather-container">
    <div class="background"></div>
    <div class="weather">
      <h1 class="title">Cuaca</h1>
      <div v-if="weatherData" class="weather-info">
        <p class="city">{{ weatherData.name }}</p>
        <p class="description">{{ weatherData.weather[0].description }}</p>
        <p class="temperature">{{ Math.round(weatherData.main.temp) }}°C</p>
        <img :src="getWeatherIconUrl(weatherData.weather[0].icon)" :alt="weatherData.weather[0].description" />
      </div>
      <div v-else>
        <p>Loading...</p>
      </div>
      <div class="search-bar">
        <input type="text" v-model="city" placeholder="Masukkan kota" required class="form-control" />
        <button @click="fetchWeatherData" class="btn btn-primary">Cari</button>
        <p class="error-message" v-if="errorMessage">{{ errorMessage }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      weatherData: null,
      city: 'Jakarta',
      errorMessage: null,
    };
  },
  mounted() {
    this.fetchWeatherData();
  },
  methods: {
    fetchWeatherData() {
      const API_KEY = 'YOUR_API_KEY';
      const API_URL = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${API_KEY}&units=metric`;

      fetch(API_URL)
        .then(response => response.json())
        .then(data => {
          this.weatherData = data;
          this.errorMessage = null;
        })
        .catch(error => {
          console.error('Error fetching weather data:', error);
          this.weatherData = null;
          this.errorMessage = 'Error fetching weather data. Please try again later.';
        });
    },
    getWeatherIconUrl(iconCode) {
      return `https://openweathermap.org/img/wn/${iconCode}.png`;
    },
  },
};
</script>

<style scoped>
.weather-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  background-image: url(data:image/jpeg;base64,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);
  background-size: cover;
  background-position: center;
  font-family: Arial, sans-serif;
}

.weather {
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 8px;
  padding: 24px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.title {
  font-size: 24px;
  font-weight: bold;
  margin-bottom: 16px;
}

.weather-info {
  margin-top: 24px;
}

.city {
  font-size: 18px;
  font-weight: bold;
}

.description {
  font-size: 14px;
  color: #888;
  margin-top: 8px;
}

.temperature {
  font-size: 24px;
  margin-top: 16px;
}

img.weather-icon {
  width: 80px;
  margin-top: 16px;
}

.search-bar {
  margin-top: 24px;
}

input[type="text"] {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-right: 8px;
}

button {
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.error-message {
  color: #ff0000;
  margin-top: 16px;
}
</style>
