<template>
  <div class="preloader"></div>
</template>

<script>
export default {
  name: 'preloader'
}
</script>

<style lang="scss">
.preloader {
  display: block;
  position: relative;
  width: 80px;
  height: 80px;
  margin: 30px auto;
  border-radius: 50%;
  border: 2px solid transparent;
  border-top-color: #4BBBFA;
  animation: preloader-spin 2s linear infinite;
}
.preloader:before {
  content: "";
  position: absolute;
  top: 5px;
  left: 5px;
  right: 5px;
  bottom: 5px;
  border-radius: 50%;
  border: 2px solid transparent;
  border-top-color: #B3B3B3;
  animation: preloader-spin 3s linear infinite;
}
.preloader:after {
  content: "";
  position: absolute;
  top: 15px;
  left: 15px;
  right: 15px;
  bottom: 15px;
  border-radius: 50%;
  border: 2px solid transparent;
  border-top-color: #4BBBFA;
  animation: preloader-spin 1.5s linear infinite;
}
@keyframes preloader-spin {
  0%   {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>