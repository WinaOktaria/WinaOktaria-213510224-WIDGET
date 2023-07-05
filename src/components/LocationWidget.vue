<template>
  <div class="location">
    <h1>Lokasi</h1>
    <div v-if="locationData">
      <p>Latitude: {{ locationData.latitude }}</p>
      <p>Longitude: {{ locationData.longitude }}</p>
      <p>Alamat: {{ locationData.address }}</p>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
    <button @click="fetchLocationData" class="btn-refresh">Refresh Lokasi</button>
    <div id="map"></div>
  </div>
</template>

<script>
export default {
  name: 'Location',
  data() {
    return {
      locationData: null,
    };
  },
  mounted() {
    this.fetchLocationData();
  },
  methods: {
    async fetchLocationData() {
      try {
        if (navigator.geolocation) {
          const position = await new Promise((resolve, reject) => {
            navigator.geolocation.getCurrentPosition(resolve, reject);
          });
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;

          const apiKey = '92591005a7b94008909d59a64b6d2a49';
          const apiUrl = `https://api.opencagedata.com/geocode/v1/json?q=${encodeURIComponent(
            latitude + ',' + longitude
          )}&key=${apiKey}`;

          const response = await fetch(apiUrl);
          const data = await response.json();

          this.locationData = {
            latitude,
            longitude,
            address: data.results[0].formatted,
          };

          this.displayMap(latitude, longitude);
        } else {
          console.error('Geolocation is not supported by this browser.');
        }
      } catch (error) {
        console.error('Error fetching location data:', error);
      }
    },
    displayMap(latitude, longitude) {
      const mapOptions = {
        center: { lat: latitude, lng: longitude },
        zoom: 10,
      };
      const mapElement = document.getElementById('map');
      const map = new google.maps.Map(mapElement, mapOptions);

      const marker = new google.maps.Marker({
        position: { lat: latitude, lng: longitude },
        map: map,
      });
    },
  },
};
</script>

<style>
.location {
  text-align: center;
  color: #fff;
  width: 400px;
  height: 400px;
  margin: 0 auto;
  border-color: black;
  background: url('https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExMXN6ZG96aTMwcmxlZ2tkYmRvb3E5YjF6bHg3bWZldDZqaHN2bXplcyZlcD12MV9naWZzX3NlYXJjaCZjdD1n/mf8UbIDew7e8g/giphy.gif');
  background-size: cover;
  background-position: center;
}

.btn-refresh {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  cursor: pointer;
  margin-top: 20px;
}

.btn-refresh:hover {
  background-color: #0056b3;
}

#map {
  height: 400px;
  width: 100%;
  margin-top: 20px;
}
</style>
