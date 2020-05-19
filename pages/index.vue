<template>
  <section class="container">
    <div>
      <app-logo/>
      <h1 class="title">
        nuxt-hi
      </h1>
      <h2 class="subtitle">
        Nuxt.js project1
      </h2>

      <div class="container" id="scanIdCardPage">
        <div class="scanIdCardDiv">
                <div class="scanCardContainer" >
                    <video ref="video" id="video" autoplay></video>
                    <canvas ref="canvas" id="canvas" width="320" height="240" style="display: none;"></canvas>
                </div>
            </div>
        </div>

        <div class="takePhotoBtnDiv">
            <div>
                <button type="button" class="btn btn-info" @click="camera('environment')">Back Camera</button>
                <button type="button" class="btn btn-info" @click="camera('user')">front Camera</button>
            </div>
        </div>
    </div>

      
    </div>
  </section>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'

export default {
  components: {
    AppLogo,
  },

  data: () => {
    return {
      video: {},
      front: true
    }
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
              this.localstream = stream;
          });
      }
      if(face === 'environment') {
          return navigator.mediaDevices.getUserMedia({video: {facingMode: {exact: face}}})
          .then(stream => {
              video.srcObject = stream;
              this.localstream = stream;
          });
      }
    }
    },
    mounted() {
      this.camera('environment');
    },
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.preview {
  display: flex;
  justify-content: center;
  align-items: center;
}

.preview img {
  max-width: 100%;
  max-height: 500px;
}
</style>

