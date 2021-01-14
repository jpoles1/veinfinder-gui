<template>
  <div class="home">
    <video id="videoInput" width="600" height="600"></video>
    <canvas id="canvasOutput" />
  </div>
</template>

<script>
import * as cv from "@/opencv.js";

const height = 600;
const width = 600;
cv["onRuntimeInitialized"] = () => {
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
  const cap = new cv.VideoCapture(video);
  const FPS = 30;
  function processVideo() {
    const begin = Date.now();
    cap.read(src);
	cv.cvtColor(src, dst, cv.COLOR_RGBA2GRAY, 0);
	cv.threshold(dst, dst, 20, 250, cv.THRESH_BINARY_INV)
	//cv.adaptiveThreshold(dst, dst, 200, cv.ADAPTIVE_THRESH_GAUSSIAN_C, cv.THRESH_BINARY, 3, 2);
    cv.imshow("canvasOutput", dst);
    // schedule next one.
    const delay = 1000 / FPS - (Date.now() - begin);
    setTimeout(processVideo, delay);
  }
  // schedule first one.
  setTimeout(processVideo, 0);
};

import Vue from "vue";
export default Vue.extend({
  name: "Home",
  components: {}
});
</script>
