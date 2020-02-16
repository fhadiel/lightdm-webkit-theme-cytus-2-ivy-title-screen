<template>
  <div class="base">
    <div
      class="bg"
      :class="{ blured: isBlured() }"
      :style="{ 'background-image': 'url(' + background + ') ; display : none'}"
    ></div>
    <video style="v-index:-1" autoplay muted loop id="idle" class="backgroundvid">
      <source src="../assets/cytus-title-ivy/1.mp4" type="video/mp4" />
    </video>
    <video ref="authed" muted style="display:none;" id="loggedin" class="backgroundvid">
      <source src="../assets/cytus-title-ivy/2.mp4" type="video/mp4" />
    </video>
    <transition name="fade">
      <router-view />
    </transition>
  </div>
</template>

<script>
import { background } from "../themer";

export default {
  name: "l-base",

  data() {
    return {
      background
    };
  },
  beforeMount() {
    window.AEsir = callback => {
      console.log("sa");
      const e = this.$refs.authed;
      e.style.display = "block";
      e.addEventListener("play", () => {
        console.log("playing");
        e.addEventListener("ended", () => {
          console.log("ended");
          callback();
        });
      });
      e.play();
    };
  },
  methods: {
    isBlured() {
      let name = this.$router.currentRoute.name;
      return name === "login" || name === "select";
    }
  }
};
</script>

<style lang="scss">
@import "../theme";
.backgroundvid {
  position: absolute;
  z-index: -1;
  top: 0;
  left: 0;
  width: 100%; 
  height: 100%;
  object-fit: cover;
}
.bg {
  background-size: cover;
  position: absolute;
  left: 0;
  right: 0;

  display: block;
  width: 100%;
  height: 100vh;

  z-index: -1;

  filter: blur(0px);
  transition: filter 500ms ease-in-out;

  color: $secondary-color;
}

.blured {
  filter: blur(10px);
}
</style>
