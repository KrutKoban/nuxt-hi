<template>
  <section class="container">
    <div>
      <video ref="video" id="video" autoplay></video>

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
    return {}
  },
  methods: {
    camera(face) {
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
              video.srcObject = stream;
          });
      }
      if(face === 'environment') {
          return navigator.mediaDevices.getUserMedia({video: {facingMode: {exact: face}}})
          .then(stream => {
              video.srcObject = stream;
          })
          .catch(e => {
            console.log(e);
          });
      }
    }
  },

  mounted() {
    this.camera('environment');
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
}
</style>

