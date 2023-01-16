<template>
  <div>
    <v-row justify="center" align="center">
      <v-col cols="12" sm="8" md="6">
        <div>
          <v-btn color="primary" dark @click="handleModal">Open Dialog</v-btn>
        </div>
      </v-col>
    </v-row>

    <v-dialog v-model="dialog" fullscreen transition="dialog-bottom-transition">
      <v-card>
        <canvas id="canvas"></canvas>
        <v-btn v-if="!openCamera" @click="accessCamera">openCamera</v-btn>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      dialog: false,
      openCamera: false,
    }
  },
  mounted() {},
  methods: {
    handleModal() {
      this.dialog = true
    },
    accessCamera() {
      let canvas = document.getElementById('canvas')
      let canvasctx = canvas.getContext('2d')
      let video = document.createElement('video')
      navigator.getUserMedia =
        navigator.getUserMedia ||
        navigator.webkitGetUserMedia ||
        navigator.mozGetUserMedia
      if (navigator.getUserMedia) {
        navigator.mediaDevices
          .getUserMedia({ video: { width: 1280, height: 720 } })
          .then((stream) => {
            console.log(window.innerHeight, window.innerWidth)
            video.srcObject = stream
            video.width = window.innerWidth
            video.height = window.innerHeight
            let self = this
            let count = 0
            video.onloadedmetadata = function (e) {
              for (let i = 0; i <= 10; i++) {
                video.play()
                canvas.width = video.width
                canvas.height = video.height
                self.openCamera = true
                canvasctx.drawImage(
                  video,
                  0,
                  0,
                  window.innerWidth,
                  window.innerHeight,
                )
                console.log(i, 'ininya')
                if (i === 10) {
                  video.remove()
                  video.srcObject = null
                  // setTimeout(() => {
                  self.accessCamera()
                  // }, 500)
                }
                setTimeout(() => {
                  requestAnimationFrame(() => {
                    self.accessCamera()
                  })
                }, 1000 / 30)
              }
            }
          })
          .catch((err) => {
            alert('The following error occurred: ' + err.name)
          })
      } else {
        alert('getUserMedia not supported')
      }
    },
  },
}
</script>
