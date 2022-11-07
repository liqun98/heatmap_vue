<template>
  <div id="app" style="display: flex; position: relative">
    <img src="./assets/bg.jpg" style="width: 100%" />
    <img v-if="img_url" :src="img_url" class="pic" />
  </div>
</template>

<script>
import axios from "axios";

function getImg(that) {
  axios
    .get("http://localhost:4000", { responseType: "arraybuffer" })
    .then(response => {
      let blob = new Blob([response.data], {
        type: response.headers["content-type"]
      });
      let image = URL.createObjectURL(blob);
      that.img_url = image;
    });
}

export default {
  name: "App",
  created() {
    const that = this;
    getImg(that);
    this.intervalId = setInterval(() => {
      getImg(that);
    }, 3000);
  },
  destroyed() {
    clearInterval(this.intervalId)
  },
  data() {
    return {
      img_url: "",
      intervalId: 0,
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.pic {
  position: absolute;
  width: 100%;
}
</style>
