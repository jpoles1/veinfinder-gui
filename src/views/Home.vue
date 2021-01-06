<template>
  <div class="home">
    assas
  </div>
</template>

<script lang="ts">
import "@/opencv";
const height = 400;
const width = 400;

const video = document.getElementById("videoInput"); // video is the id of video tag
navigator.mediaDevices
  .getUserMedia({ video: true, audio: false })
  .then(function(stream) {
    video.srcObject = stream;
    video.play();
  })
  .catch(function(err) {
    console.log("An error occurred! " + err);
  });

const src = new cv.Mat(height, width, cv.CV_8UC4);
const dst = new cv.Mat(height, width, cv.CV_8UC1);
const cap = new cv.VideoCapture(videoSource);
const FPS = 30;
function processVideo() {
  const begin = Date.now();
  cap.read(src);
  cv.cvtColor(src, dst, cv.COLOR_RGBA2GRAY);
  cv.imshow("canvasOutput", dst);
  // schedule next one.
  const delay = 1000 / FPS - (Date.now() - begin);
  setTimeout(processVideo, delay);
}
// schedule first one.
setTimeout(processVideo, 0);

import Vue from "vue";
export default Vue.extend({
  name: "Home",
  components: {}
});
</script>
