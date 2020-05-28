<template>
  <section class="container">
    <div>
      <div v-show="cameraRun" class="video-caontainer">
        <video ref="video" id="video" autoplay></video>
        <div
          class="video-inner-block"
          ref="red_block"
        >
          <video ref="video2" id="video2" autoplay></video>
        </div>
      </div>

      <div class="photo-buttons">
        <button type="button" class="btn-back" @click="camera('environment')">Back Camera</button>
        <button type="button" class="btn-front" @click="camera('user')">front Camera</button>
      </div>
    </div>
  </section>
</template>

<script>

export default {
  data: () => {
    return {
      cameraRun: false
    }
  },
  methods: {
    onMouseDown(e) {
      let ball = this.$refs.red_block;

      const moveAt = (e) => {
        ball.style.left = e.pageX - ball.offsetWidth / 2 + 'px';
        ball.style.top = e.pageY - ball.offsetHeight / 2 + 'px';
      }
      moveAt(e);
      // document.body.appendChild(ball);

      ball.style.zIndex = 1000;

      

      document.onmousemove = function(e) {
        moveAt(e);
      }

      ball.onmouseup = function() {
        document.onmousemove = null;
        ball.onmouseup = null;
      }
    },

    camera(face) {
      this.cameraRun = true;
      this.stop();
      this.gum(face);
    },
    stop() {
      return video.srcObject && video.srcObject.getTracks().map(t => t.stop());
    },
    gum(face) {
      if(face === 'user') {
          return navigator.mediaDevices.getUserMedia({video: {facingMode: face}})
          .then(stream => {
              // this.localStream = stream;
              video.srcObject = stream;
              video2.srcObject = stream;
          });
      }
      if(face === 'environment') {
          return navigator.mediaDevices.getUserMedia({video: {facingMode: {exact: face}}})
          .then(stream => {
              // this.localStream = stream;
              video.srcObject = stream;
              // video2.srcObject = stream;
          })
          .catch(e => {
            console.log(e);
          });
      }
    }
  },

  mounted() {
    this.camera('environment');
    this.camera('user');
    var idx = 0;
    var filters = ['grayscale', 'sepia', 'blur', ''];

    function changeFilter(e) {
      var el = e.target;
      el.className = '';
      var effect = filters[idx++ % filters.length]; // loop through filters.
      if (effect) {
        el.classList.add(effect);
      }
    }

    document.querySelector('video').addEventListener('click', changeFilter, false);

    // const startup = () => {
      // console.log('DOMContentLoaded');
    const el = document.getElementsByClassName('video-inner-block')[0];
    console.log(el);
    el.addEventListener('touchmove', (e) => {
      var touchLocation = e.targetTouches[0];
      
      el.style.left = touchLocation.pageX + 'px';
      el.style.top = touchLocation.pageY + 'px';
    });

    el.addEventListener('touchend', (e) => {
      // current box position.
      var x = parseInt(el.style.left);
      var y = parseInt(el.style.top);
    })
    // el.addEventListener("touchmove", this.onMouseDown, false);
    // }
    // document.addEventListener("DOMContentLoaded", startup);
  },
}
</script>

<style>
@media (min-width: 320px)
  and (max-width: 425px) {
  .video-size {
    width: 100vw;
    height: 50vh;
  }

  .btn-back,
  .btn-front {
    width: 30vw;
    height: 10vh;
    border: 1px solid black;
    border-radius: 5px;
    background-color: white;
  }
}

@media (min-width: 1024px) {
  .video-size {
    width: 50vw;
    height: 50vh;
  }

  .btn-back,
  .btn-front {
    width: 10vw;
    height: 5vh;
    border: 1px solid black;
    border-radius: 5px;
    background-color: white;
  }
}

.photo-buttons {
  display: flex;
  justify-content: space-around;
}

video {
  width: 100%;  
  height: 100%;
} 

.grayscale {
  filter: grayscale(1);
}
.sepia {
  filter: sepia(1);
}
.blur {
  filter: blur(3px);
}

.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  overflow: hidden;
}

.video-container {
  position: relative;
}

.video-inner-block {
  width: 100px;
  height: 70px;
  border: 1px solid red;
  position: absolute;
  top: 50%;
  left: 50%;
}
</style>

