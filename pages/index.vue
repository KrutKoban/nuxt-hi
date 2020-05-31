<template>
  <section class="container">
    <div>
      <div v-show="cameraRun" class="video-caontainer">
        <video class="video-size" ref="video" id="video" autoplay></video>
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
      cameraRun: false,
      idx: 0,
      filters: ['grayscale', 'sepia', 'blur', '']
    }
  },

  computed: {
    movingCamera() {
      return this.$refs.video2;
    }
  },

  methods: {
    changeFilter(e) {
      const el = e.target;
      el.className = '';
      const effect = this.filters[this.idx++ % this.filters.length]; // loop through filters.
      if (effect) {
        el.classList.add(effect);
      }
    },

    onTouchMove(e) {
      const touchLocation = e.targetTouches[0];
      const movCamera = this.movingCamera;
      
      movCamera.style.left = touchLocation.pageX + 'px';
      movCamera.style.top = touchLocation.pageY + 'px';
    },

    onMouseDown(e) {
      const movCamera = this.movingCamera;

      const moveAt = (e) => {
        movCamera.style.left = e.pageX - movCamera.offsetWidth / 2 + 'px';
        movCamera.style.top = e.pageY - movCamera.offsetHeight / 2 + 'px';
      }
      moveAt(e);

      movCamera.style.zIndex = 1000;

      document.onmousemove = function(e) {
        moveAt(e);
      }

      movCamera.onmouseup = function() {
        document.onmousemove = null;
        movCamera.onmouseup = null;
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
              // video.srcObject = stream;
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

    document.querySelector('video').addEventListener('click', this.changeFilter, false);

    this.movingCamera.addEventListener('touchmove', this.onTouchMove);
    this.movingCamera.addEventListener('mousedown', this.onMouseDown);
  },
}
</script>

<style>
@media (min-width: 320px) and (max-width: 1024px) {
  #video2 {
    width: 30vw;
    height: 17vh;
    position: absolute;
    top: 50%;
    left: 50%;
  }

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
  #video2 {
    width: 15vw;
    height: 30vh;
    position: absolute;
    top: 50%;
    left: 50%;
  }

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

/*video {
  width: 100%;  
} */

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
}

.video-container {
  position: relative;
}
</style>

