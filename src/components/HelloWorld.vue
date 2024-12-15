<script>
import axios from 'axios';

export default {
  data() {
    return {
      imageFile: null, 
      uploadedImageUrl: '', 
      imageList: [] 
    };
  },
  methods: {
    handleFileChange(event) {
      this.imageFile = event.target.files[0]; 
    },
    async saveImage() {
      if (!this.imageFile) {
        console.log('No file selected');
        return;
      }

      const imageUrl = `http://localhost:3000/data/${this.imageFile.name}`;

      // Here, we are not uploading the image; we just create the URL
      try {
        const response = await axios.post('http://localhost:3000/data', {
          imageUrl,
        });

        this.uploadedImageUrl = response.data.imageUrl; 
        console.log('Image uploaded successfully:', this.uploadedImageUrl);
        
        this.fetchImages();
      } catch (error) {
        console.error('Error uploading image:', error.response?.data || error.message);
      }
    },
    async fetchImages() {
      try {
        const response = await axios.get('http://localhost:3000/data');
        this.imageList = response.data;
      } catch (error) {
        console.error('Error fetching images:', error.message);
      }
    },
  },
  created() {
    this.fetchImages(); 
  }
};
</script>

<template>
  <div>
    <div class="mb-3">
      <label for="formFile" class="form-label">Upload Image</label>
      <input 
        class="form-control" 
        type="file" 
        id="formFile" 
        @change="handleFileChange" 
      >
    </div>

    <button type="button" class="btn btn-primary" @click="saveImage">
      Submit
    </button>

    <div v-if="uploadedImageUrl" class="mt-3">
      <h5>Uploaded Image:</h5>
      <img :src="uploadedImageUrl" alt="Uploaded" style="max-width: 100%; height: auto;">
    </div>

    <div v-if="imageList.length" class="mt-5">
      <h5>All Uploaded Images:</h5>
      <div v-for="(image, index) in imageList" :key="index">
        <img :src="image.imageUrl" alt="Uploaded" style="max-width: 100%; height: auto; margin-top: 10px;">
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Add any additional styles */
</style>
