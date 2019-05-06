# p5.js Video on IOS - MCVE
Simple example on how to implement video on IOS mobile browsers

see https://discourse.processing.org/t/ios-support-for-inline-video-touch-events-and-audio/10961/1 for more info

try it on your IOS device:

![qr_code](assets/qr.png)

```javascript
// free video countdown videod from:
// https://www.videvo.net/video/pink-countdown-timer/3683/
// the video is garbage and never gets to 2,
// I thought it was a problem with the code bit it's not

var videoP;
var video;
let slide;
let target;
var video;
var isIphone;
var started = false;

// Last update timestamp
let then;
// Flag on if playing
let busy = false;

// Advance video playhead before rendering
function update(now) {
  const step = now - (then || now)
  if (busy) {
    video.currentTime += step * 0.001
    // Reset
    if (video.currentTime >= video.duration) {
      video.currentTime = 0
      busy = false
    }
  }
  // Reset
  then = now;
}

function preload () {
    videoP = 'assets/countdown.mp4';
    video = null;
    isIphone = window.navigator.userAgent.match(/iPad/i) || window.navigator.userAgent.match(/iPhone/i);

    if (isIphone) {
      then = Date.now();
      video = document.createElement('video');
      video.setAttribute('src', videoP);
      video.load();
    }
    else {
      video = createVideo(videoP);
      video.hide();
    }
}


function setup() {
  createCanvas(window.innerWidth, window.innerHeight);
  target = document.querySelector('canvas').getContext('2d');

  slide = 0;

  if (isIphone) {
    var el = document.getElementsByTagName("canvas")[0];
    el.addEventListener("touchstart", mouseClicked, false);
    el.addEventListener('touchstart', start, false)
  }

  textSize(32);
  textAlign(CENTER, CENTER);
}



function draw() {

  if (slide == 0 ) {
    background(0);
    fill(255);
    text('start Video', 0,0, width, height);

  } else if (slide == 1) {
    if (isIphone) {
      busy = true;
      update(Date.now());
      target.drawImage(video, 0, 0,width,height);
    } else {
      if (video != null) {
        image(video,0,0,width,height);
      }
    }

    fill(0);
    text('Video', 0,0, width, height);
  }

}

function mouseClicked() {

  if (slide == 1) {
    slide=0;
  } else if (slide == 0 ) {
    if (!isIphone) {
      ( !fullscreen() ? fullscreen(true) : null )
      if (video != null) {
        video.loop();
      }
    }
    slide++;
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

```
