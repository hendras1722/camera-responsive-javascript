<template>
  <div>
    <v-text-field
      v-model="cek"
      label="Outlined"
      placeholder="Placeholder"
      outlined
      @keypress="handleKeyPress"
      inputmode="numeric"
    ></v-text-field>
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
      cek: '',
    }
  },
  mounted() {},
  methods: {
    handleKeyPress(e) {
      console.log(e, 'inienya')
      if (!`${e.target.value}${e.key}`.match(/^[0-9]{0,}$/)) {
        // block the input if result does not match
        e.preventDefault()
        e.stopPropagation()
        return false
      }
    },
    handleModal() {
      this.dialog = true
    },
    accessCamera() {
      let canvas = document.getElementById('canvas')
      let canvasctx = canvas.getContext('2d')
      let video = document.createElement('video')

      if (navigator.getUserMedia) {
        navigator.mediaDevices
          .getUserMedia({ video: true })
          .then((stream) => {
            video.srcObject = stream
            video.width = window.innerWidth
            video.height = window.innerHeight
            let self = this
            let count = 0
            video.onloadedmetadata = function (e) {
              // for (let i = 0; i <= 10; i++) {
              video.play()
              self.openCamera = true
              setInterval(() => {
                canvas.width = window.innerWidth
                canvas.height = window.innerHeight
                canvasctx.drawImage(
                  video,
                  0,
                  0,
                  window.innerWidth,
                  window.innerHeight,
                )
              }, 10)
              // console.log('i', 'ininya')
              // if (i === 10) {
              // video.remove()
              // video.srcObject = null
              // setTimeout(() => {
              // self.accessCamera()
              // }, 500)
              // }
              // setTimeout(() => {
              //   requestAnimationFrame(() => {
              //     self.accessCamera()
              //   })
              // }, 1000 / 20)
              // }
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
