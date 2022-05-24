<template>
  <div class="covered-pic">
    <img src="../assets/img/covered_picture.png" alt="" />
    <div class="text-container">
      <div class="text">
        <h1><span>Now</span> Streaming</h1>
        <div class="container-txf">
          <v-text-field
            v-model="message"
            label="Search movie"
            solo
            style="max-width: 50%"
            prepend-inner-icon="mdi-magnify"
            :append-icon="message != '' ? 'mdi-close' : undefined"
            @click:append="onClear"
          ></v-text-field>
        </div>
        <div>
          <v-btn color="info" x-large> View Movies </v-btn>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Hero',
  data: () => ({
    message: '',
  }),
  methods: {
    onClear() {
      this.message = ''
    },
  },
  watch: {
    message() {
      this.$emit('change', this.message)
    },
  },
}
</script>

<style>
.container-txf {
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.text {
  display: flex;
  width: 50%;

  flex-direction: column;
  justify-content: center;
}
.covered-pic {
  height: 400px;
  position: relative;
  text-align: center;
  display: flex;
  justify-content: center;
}

.text-container {
  display: flex;
  justify-content: center;
  z-index: 99;
  position: absolute;
  align-self: center;
  width: 60%;
  top: 0;
  margin: 0;

  height: 100%;
  flex-direction: column;
  justify-content: center;
}

.text {
  width: 100%;
  text-align: center;
}
</style>