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
        <div @click="accessCamera">openCamera</div>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: 'InspirePage',
}
</script>
