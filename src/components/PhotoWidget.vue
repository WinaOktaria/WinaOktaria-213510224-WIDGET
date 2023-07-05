<template>
  <div>
    <div class="photo">
      <h1>Foto</h1>
      <input type="file" @change="uploadPhoto" accept="image/*" />
      <div v-if="photos.length > 0" class="photo-slider">
        <transition-group name="slide" mode="out-in">
          <div v-for="(photo, index) in photos" :key="index" class="photo-slide">
            <img :src="photo.url" :alt="photo.name" />
            <p>{{ photo.name }}</p>
          </div>
        </transition-group>
      </div>
    </div>

    <div class="photo-widget">
      <h2>{{ title }}</h2>
      <img :src="photoUrl" :alt="title" />
      <button @click="getRandomPhoto">Load New Photo</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Photo',
  data() {
    return {
      photos: [],
      title: '',
      photoUrl: '',
    };
  },
  mounted() {
    this.getRandomPhoto();
  },
  methods: {
    async getRandomPhoto() {
      try {
        const apiKey = '38037020-2c48722c03be8437a05b588e6';
        const apiUrl = `https://pixabay.com/api/?key=${apiKey}&q=nature&image_type=photo&orientation=horizontal`;

        const response = await fetch(apiUrl);
        const data = await response.json();

        const randomIndex = Math.floor(Math.random() * data.hits.length);
        const randomPhoto = data.hits[randomIndex];

        this.title = randomPhoto.tags;
        this.photoUrl = randomPhoto.webformatURL;
      } catch (error) {
        console.error('Error fetching random photo:', error);
      }
    },
    uploadPhoto(event) {
      const files = event.target.files;
      for (let i = 0; i < files.length; i++) {
        const file = files[i];
        const reader = new FileReader();

        reader.onload = () => {
          this.photos.push({
            name: file.name,
            url: reader.result,
          });
        };

        reader.readAsDataURL(file);
      }
    },
  },
};
</script>

<style scoped>
.photo {
  text-align: center;
}

.photo-slider {
  margin-top: 20px;
}

.photo-slide {
  display: inline-block;
  max-width: 300px;
  max-height: 300px;
  margin-right: 10px;
  animation: slideAnimation 0.5s;
}

img {
  max-width: 100%;
  max-height: 100%;
}

p {
  margin-top: 10px;
}

.slide-enter-active,
.slide-leave-active {
  transition: opacity 0.5s;
}

.slide-enter,
.slide-leave-to {
  opacity: 0;
}

.photo-widget {
  border: 1px solid #ccc;
  padding: 20px;
  margin-bottom: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.photo-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  max-width: 500px;
  margin-top: 10px;
}

.photo-container img {
  max-width: 100%;
  max-height: 100%;
}

.photo-widget button {
  margin-top: 10px;
}
</style>
