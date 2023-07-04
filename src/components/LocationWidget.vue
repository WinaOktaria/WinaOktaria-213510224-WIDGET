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
    fetchLocationData() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
          const latitude = position.coords.latitude;
          const longitude = position.coords.longitude;

          // Ganti API_KEY dengan kunci API Nominatim (untuk reverse geocoding)
          const API_KEY = 'eeWoXNeWZJKz_Um8wJFakDt7EHmmKj2BPGVyzCZUJFg';
          const API_URL = `https://geocode.search.hereapi.com/v1/geocode?q=240+Washington+St.%2C+Boston&limit=4&apiKey={eeWoXNeWZJKz_Um8wJFakDt7EHmmKj2BPGVyzCZUJFg}}`;

          fetch(API_URL)
            .then(response => response.json())
            .then(data => {
              this.locationData = {
                latitude,
                longitude,
                address: data.display_name,
              };
              this.displayMap(latitude, longitude);
            })
            .catch(error => console.error('Error fetching location data:', error));
        });
      } else {
        console.error('Geolocation is not supported by this browser.');
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
