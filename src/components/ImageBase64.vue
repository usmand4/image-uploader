<template>
  <div>
      <h1 class="heading">File Upload with Base64</h1>

      <div class="file-upload-container">
          <input type="file" @change="handleFileChange" accept="image/*" class="file-input">
          <br>
          <button @click="uploadFile" class="upload-button">Upload File</button>
      </div>
      <div class="imageCapture">
          <h1>Media Capture</h1>
          <video id="video" autoplay></video>
          <canvas id="canvas"></canvas>
          <button @click="captureImage" id="captureButton">Capture Image</button>
      </div>
      <div class="getImage">
          <div v-for="(image) in images" :key="image.id" class="image-container">
              {{ console.log(image.path) }}
              <img :src="getImageUrl(image.path)" alt="Image not found" class="image">
          </div>
      </div>


  </div>
</template>
<script>
import axios from 'axios';
export default {
  data() {
      return {
          selectedFile: null,
          base64String: null,
          video: null,
          canvas: null,
          images: [],

      };
  },
 
  mounted() {
      this.showImage();
      const video = document.getElementById('video');
      const canvas = document.getElementById('canvas');

      this.video = video;
      this.canvas = canvas;

      navigator.mediaDevices.getUserMedia({ video: true })
          .then(stream => {
              video.srcObject = stream;
          })
          .catch(error => {
              console.error('Error accessing media devices:', error);
          });
  },
  methods: {
      handleFileChange(event) {
          this.selectedFile = event.target.files[0];

          if (this.selectedFile) {
              this.readFile();
          }
      },
      readFile() {
          const reader = new FileReader();

          reader.onloadend = () => {
              this.base64String = reader.result;
          };

          reader.readAsDataURL(this.selectedFile);
      },
      uploadFile() {
          if (this.base64String) {
              const formData = new FormData();
              formData.append('base64String', this.base64String);

              // console.log('base64String', this.base64String);

              axios.post('http://10.0.10.84:8000/api/image', formData)
                  .then(response => {
                      console.log('Server response:', response.data);
                  })
                  .catch(error => {
                      console.error('Error uploading file:', error);

                  });
          } else {
              console.error('No file selected.');
          }
      },
      showImage() {
          axios.get('http://10.0.10.84:8000/api/image')
              .then(response => {
                  console.log('Received images data:', response.data);

                  this.images = response.data;
                  console.log(this.images);


              })
              .catch(error => {
                  console.error('Error fetching images:', error);
              });
      },
      getImageUrl(path) {
          const baseUrl = 'http://10.0.10.84:8000/storage/';

          if (path) {

              const imageUrl = `${baseUrl}${path}`;
              console.log('Image URL:', imageUrl);
              return imageUrl;
          } else {
              console.error('Image path is undefined');
              return '';
          }
      },


      captureImage() {
          const video = this.video;
          const canvas = this.canvas;

          if (!video || !canvas) {
              console.error('Video or canvas element not found');
              return;
          }

          const context = canvas.getContext('2d');

          if (!context) {
              console.error('2D context not supported');
              return;
          }

          canvas.width = video.videoWidth;
          canvas.height = video.videoHeight;
          context.drawImage(video, 0, 0, canvas.width, canvas.height);

          this.base64String = canvas.toDataURL('image/*');
          console.log('base64String', this.base64String);

          this.sendImageToServer(this.base64String);
      },

      sendImageToServer(base64String) {
          const formData = new FormData();
          formData.append('base64String', base64String);

          axios.post('http://10.0.10.84:8000/api/image', formData)
              .then(response => {
                  console.log('Server response:', response.data);
              })
              .catch(error => {
                  console.error('Error uploading captured image:', error);
              });
      },


  },
};
</script>
<style scoped>
.heading {
  text-align: center;
  color: #333;
}

.file-upload-container {
  margin: 20px;
  text-align: center;
}

.file-input {
  margin-bottom: 10px;

}

.upload-button {
  padding: 10px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.upload-button:hover {
  background-color: #45a049;
}

.imageCapture {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
  margin: 0;
  padding: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

video {
  width: 100%;
  max-width: 400px;
}

canvas {
  display: none;
}

button {
  background-color: #4caf50;
  color: #fff;
  padding: 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #45a049;
}

.getImage {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}

.image-container {
  margin: 10px;
}

.image {
  width: 200px; 
    height: 150px; 
    object-fit: cover;
}
</style>