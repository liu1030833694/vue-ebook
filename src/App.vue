<template>
  <div id="app">
    <router-view />
  </div>
</template>
<script>

export default {
  name: 'APP'
}
document.addEventListener('DOMContentLoaded', () => {
  const html = document.querySelector('html')
  let fontSize = window.innerWidth / 10
  console.log(fontSize)
  fontSize = fontSize > 50 ? 50 : fontSize
  console.log(fontSize)
  html.style.fontSize = fontSize + 'px'
})
</script>
<style lang='scss' scoped>
@import "./assets/styles/global";
#app {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
</style>
