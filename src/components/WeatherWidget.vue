<template>
  <div class="weather-widget">
    <h2 class="widget-title">Weather Widget</h2>
    <div class="location-input">
      <label for="location">Enter Location:</label>
      <input type="text" id="location" v-model="location" />
      <button @click="fetchWeatherData">Get Weather</button>
    </div>
    <div v-if="weatherData" class="weather-data">
      <p class="location1">Location: {{ weatherData.name }}</p>
      <p v-if="weatherData.main" class="temperature">
        Temperature: {{ weatherData.main.temp }}°C
      </p>
      <p v-if="weatherData.weather" class="description">
        Description: {{ weatherData.weather[0].description }}
      </p>
    </div>
    <p v-else class="loading-message">Loading weather data...</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: '',
      weatherData: null
    };
  },
  methods: {
    async fetchWeatherData() {
      try {
        const apiKey = 'b7bfca7b27a3485144fea086c50d09dc';
        const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.location}&appid=${apiKey}`;
        const response = await fetch(apiUrl);
        const data = await response.json();
        this.weatherData = data;
      } catch (error) {
        console.error('Error fetching weather data:', error);
      }
    }
  }
};
</script>

<style>
body{
  background-color: #FFD0D0;
}
.weather-widget {
  width: 400px; /* Lebar kotak */
  height: 400px; /* Tinggi kotak */
  background-image: url('https://github.com/TylerPottsDev/weather-vue/blob/master/src/assets/cold-bg.jpg?raw=true'); /* Ganti path_to_your_image.jpg dengan path file gambar Anda */
  background-size: cover;
  background-position: center;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  margin: 0 auto; /* Mengatur posisi widget menjadi tengah */
  border-color: black;
}
.widget-title {
  font-size: 24px;
  text-align: center;
  margin-bottom: 16px;
}

.location-input {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 16px;
}

.location-input label {
  font-size: 18px;
  margin-right: 8px;
}

.location-input input {
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
  outline: none;
}

.location-input button {
  padding: 8px 16px;
  border-radius: 4px;
  border: none;
  background-color: #4caf50;
  color: #fff;
  font-size: 16px;
  cursor: pointer;
}

.weather-data {
  margin-top: 16px;
}

.location {
  font-size: 18px;
  font-weight: bold;
  margin-bottom: 8px;
}

.temperature {
  font-size: 24px;
  margin-bottom: 8px;
}

.description {
  font-size: 18px;
}

.loading-message {
  text-align: center;
  font-size: 18px;
  margin-top: 16px;
}
</style>
