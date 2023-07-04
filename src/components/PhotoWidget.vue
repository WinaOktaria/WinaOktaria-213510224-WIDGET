<template>
    <div class="photo">
      <h1>Foto</h1>
      <input type="file" @change="uploadPhoto" accept="image/*" />
      <div v-if="photoUrl">
        <img :src="photoUrl" :alt="uploadedPhotoName" />
        <p>{{ uploadedPhotoName }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Photo',
    data() {
      return {
        photoUrl: null,
        uploadedPhotoName: null,
      };
    },
    methods: {
      uploadPhoto(event) {
        const file = event.target.files[0];
        this.uploadedPhotoName = file.name;
  
        const reader = new FileReader();
        reader.onload = () => {
          this.photoUrl = reader.result;
        };
        reader.readAsDataURL(file);
      },
    },
  };
  </script>
  
  <style>
  .photo {
    text-align: center;
  }
  
  img {
    max-width: 300px;
    max-height: 300px;
    margin-top: 20px;
  }
  
  p {
    margin-top: 10px;
  }
  </style>
  