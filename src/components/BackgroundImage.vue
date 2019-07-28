<template>
  <div class="backgroundImage" />
</template>

<script>
export default {
  name: "BackgroundImage"
};
</script>

<style lang="scss" scoped>
.backgroundImage {
  position: fixed;
  top: 0;
  z-index: -1;
  height: 100vh;
  width: 100%;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: 80% 0%;
  background-image: url("../assets/heroimage.jpg");
}
.backgroundImage::after{
    content:'';
    background: rgba($color: #000000, $alpha: .3);
    position:absolute;
    top:0;
    left:0;
    z-index: 1;
    width: 100%;
    height: 100vh;
}
</style>
