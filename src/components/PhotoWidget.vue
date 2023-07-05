<template>
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
</template>

<script>
export default {
  name: 'Photo',
  data() {
    return {
      photos: [],
    };
  },
  methods: {
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

<style>
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
</style>
