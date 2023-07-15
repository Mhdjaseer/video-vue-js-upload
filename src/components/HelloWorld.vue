<template>
  <div class="container">
    <form @submit.prevent="uploadVideo">
      <h2>Upload Video</h2>
      <div class="form-group">
        <label for="title">Title:</label>
        <input type="text" v-model="title" id="title" placeholder="Title" required>
      </div>
      <div class="form-group">
        <label for="videoFile">Video:</label>
        <input type="file" ref="videoFile" id="videoFile" required>
      </div>
      <div class="form-group">
        <label for="time">Time:</label>
        <input type="datetime-local" v-model="time" id="time" required>
      </div>
      <button type="submit">Upload Video</button>
    </form>
    <div v-if="response" class="response">
      <h3>Response:</h3>
      <p>ID: {{ response.id }}</p>
      <p>Title: {{ response.title }}</p>
      <p>Video URL: <a href="response.vedio">{{ response.vedio }}</a></p>
      <div class="video-container">
        <video controls>
          <source :src="response.vedio" type="video/mp4">
          Your browser does not support the video tag.
        </video>
      </div>
      <p>Time: {{ response.time }}</p>
    </div>
  </div>
</template>

<style>
.container {
  max-width: 500px;
  margin: 0 auto;
  padding: 20px;
}

h2 {
  text-align: center;
}

form {
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 10px;
}

label {
  display: block;
}

input[type="text"],
input[type="file"],
input[type="datetime-local"] {
  width: 100%;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

button {
  display: block;
  margin: 0 auto;
  padding: 8px 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.response {
  margin-top: 20px;
  border: 1px solid #ccc;
  padding: 10px;
}

.video-container {
  max-width: 100%;
  overflow: hidden;
  position: relative;
  padding-bottom: 56.25%; /* 16:9 aspect ratio */
}

.video-container video {
  position: absolute;
  width: 100%;
  height: auto;
  top: 0;
  left: 0;
}
</style>

<script>
export default {
  data() {
    return {
      title: "",
      time: "",
      response: null
    };
  },
  methods: {
    uploadVideo() {
      const formData = new FormData();
      formData.append("title", this.title);
      formData.append("vedio", this.$refs.videoFile.files[0]);
      formData.append("time", this.time);

      fetch("http://127.0.0.1:8000", {
        method: "POST",
        body: formData
      })
        .then(response => response.json())
        .then(data => {
          this.response = data;
        })
        .catch(error => {
          console.error("Error:", error);
        });
    }
  }
};
</script>
