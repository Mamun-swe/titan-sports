<template>
  <div class="loader">
    <div class="preloader-1">
      <div>Loading</div>
      <span class="line line-1"></span>
      <span class="line line-2"></span>
      <span class="line line-3"></span>
      <span class="line line-4"></span>
      <span class="line line-5"></span>
      <span class="line line-6"></span>
      <span class="line line-7"></span>
      <span class="line line-8"></span>
      <span class="line line-9"></span>
      <span class="line line-9"></span>
      <span class="line line-9"></span>
      <span class="line line-9"></span>
      <span class="line line-9"></span>
      <span class="line line-9"></span>
    </div>
  </div>
</template>
<script>
export default {
  name: "loader"
};
</script>
<style lang="scss" scoped>
.loader {
  display: flex;
  align-items: center;
  position: relative;
  background: linear-gradient(to bottom right, #070630 0%, #060454 100%);
  z-index: 999999;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100vh;
}
.preloader-1 {
  margin: 100px auto 0;
  width: 66px;
  height: 12px;
}

div {
  color: #fff;
  margin: 5px 0;
  text-transform: uppercase;
  text-align: center;
  font-family: "Arial", sans-serif;
  font-size: 10px;
  letter-spacing: 2px;
}

.preloader-1 .line {
  width: 1px;
  height: 12px;
  background: #fff;
  margin: 0 1px;
  display: inline-block;
  animation: opacity-1 1000ms infinite ease-in-out;
}

.preloader-1 .line-1,
.preloader-2 .line-1 {
  animation-delay: 800ms;
}
.preloader-1 .line-2,
.preloader-2 .line-2 {
  animation-delay: 600ms;
}
.preloader-1 .line-3,
.preloader-2 .line-3 {
  animation-delay: 400ms;
}
.preloader-1 .line-4,
.preloader-2 .line-4 {
  animation-delay: 200ms;
}
.preloader-1 .line-6,
.preloader-2 .line-6 {
  animation-delay: 200ms;
}
.preloader-1 .line-7,
.preloader-2 .line-7 {
  animation-delay: 400ms;
}
.preloader-1 .line-8,
.preloader-2 .line-8 {
  animation-delay: 600ms;
}
.preloader-1 .line-9,
.preloader-2 .line-9 {
  animation-delay: 800ms;
}

@keyframes opacity-1 {
  0% {
    opacity: 1;
  }
  50% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>