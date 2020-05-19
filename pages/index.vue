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
      <div><video ref="video" id="video" width="640" height="480" autoplay></video></div>
      <div><button id="snap" v-on:click="capture()">Snap Photo</button></div>
      <canvas ref="canvas" id="canvas" width="640" height="480"></canvas>
      <ul>
          <li v-for="c in captures">
              <img v-bind:src="c" height="50" />
          </li>
      </ul>

      
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
			canvas: {},
			captures: []
    }
  },
  methods: {
		capture() {
			this.canvas = this.$refs.canvas;
			var context = this.canvas
				.getContext("2d")
				.drawImage(this.video, 0, 0, 640, 480);
			this.captures.push(canvas.toDataURL("image/png"));
		}
	},
  mounted() {
		this.video = this.$refs.video;
		if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
			navigator.mediaDevices.getUserMedia({ video: true, audio: false }).then(stream => {
				console.log(stream)
				video.srcObject = stream;
				this.video.play();
			});
		}
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

