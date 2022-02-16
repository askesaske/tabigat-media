<template>
  <div class="ultra-main">
    <Nuxt />
  </div>
</template>

<script>

</script>

<style lang="scss">
@import '../node_modules/swiper/swiper-bundle.css';
@import "../assets/scss/style";

@font-face {
  font-family: 'Gilroy';
  src: url('../assets/fonts/gilroy-semibold.eot');
  src: local('☺'), url('../assets/fonts/gilroy-semibold.woff') format('woff'), url('../assets/fonts/gilroy-semibold.ttf') format('truetype'), url('../assets/fonts/gilroy-semibold.svg') format('svg');
  font-weight: 600;
  font-style: normal;
}

@font-face {
  font-family: 'Gilroy';
  src: url('../assets/fonts/gilroy-regular.eot');
  src: local('☺'), url('../assets/fonts/gilroy-regular.woff') format('woff'), url('../assets/fonts/gilroy-regular.ttf') format('truetype'), url('../assets/fonts/gilroy-regular.svg') format('svg');
  font-weight: 400;
  font-style: normal;
}

@font-face {
  font-family: 'Futura PT';
  src: url('../assets/fonts/FuturaPT-Medium.eot');
  src: url('../assets/fonts/FuturaPT-Medium.eot?#iefix') format('embedded-opentype'),
  url('../assets/fonts/FuturaPT-Medium.woff') format('woff'),
  url('../assets/fonts/FuturaPT-Medium.ttf') format('truetype');
  font-weight: 400;
  font-style: normal;
}

@font-face {
  font-family: 'Futura PT';
  src: url('../assets/fonts/FuturaPT-Heavy.eot');
  src: url('../assets/fonts/FuturaPT-Heavy.eot?#iefix') format('embedded-opentype'),
  url('../assets/fonts/FuturaPT-Heavy.woff') format('woff'),
  url('../assets/fonts/FuturaPT-Heavy.ttf') format('truetype');
  font-weight: 600;
  font-style: normal;
}

.top-dot {
  position: fixed;
  top: 0;
  left: 0;
  opacity: 0;
  visibility: hidden;
}
</style>
