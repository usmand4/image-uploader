# imageUploader
Vue File Upload and Image Capture Component
This Vue.js component provides functionality for file upload with Base64 encoding and media capture using the device camera. It includes features for selecting an image file, uploading it to a server using Base64 encoding, capturing images from the device camera, and displaying a gallery of images.

Features
File Upload:

Select an image file using the file input.
Convert the selected image to Base64 format.
Upload the Base64-encoded image to the server.
Media Capture:

Use the device camera to capture images.
Display the captured image on the canvas.
Convert the captured image to Base64 format.
Upload the Base64-encoded image to the server.
Image Gallery:

Fetch and display images from the server.
Each image is displayed with a fixed width and height for a consistent user experience.
Getting Started
Clone the repository:

git clone https://github.com/your-username/vue-file-upload.git
Navigate to the project directory:


cd vue-file-upload
Install dependencies:


npm install
Run the development server:



npm run serve
The application will be available at http://localhost:8080.

Usage
File Upload:

Click the file input to select an image file.
Click the "Upload File" button to upload the selected file to the server.
Media Capture:

Access the device camera by allowing permissions.
Click the "Capture Image" button to capture an image.
The captured image will be displayed on the canvas.
Click the "Upload File" button to upload the captured image to the server.
Image Gallery:

The component automatically fetches and displays images from the server.
Images are displayed in a responsive gallery with fixed width and height.
Technologies Used
Vue.js 3
Axios for HTTP requests
HTML5 Canvas for image manipulation
MediaDevices API for accessing the device camera
Configuration
Update the server endpoints in the component methods (uploadFile, showImage, sendImageToServer) to match your backend API.
License
This project is licensed under the MIT License.