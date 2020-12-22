<template>
  <div>
    <Nuxt />
  </div>
</template>

<style lang="scss">
@import '@/assets/reset.scss';

html {
  font-family:
    'SF Pro Display',
    -apple-system,
    BlinkMacSystemFont,
    'Roboto',
    Roboto,
    sans-serif;
  font-size: 16px;
  word-spacing: 1px;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
  min-height: 100vh;
}

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

.container {
  margin: 0 auto;
  max-width: 1480px;
  padding: 0 20px;
}

</style>

<script>
import Vue from 'vue'

export default Vue.extend({})
</script>
