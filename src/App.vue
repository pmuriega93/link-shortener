<template>
  <div>
    <Header />
    <Main />
  </div>
</template>

<script>
import Header from '@/components/Header.vue'
import Main from '@/components/Main.vue'

export default {
  name: 'App',
  components: {
    Header,
    Main
  }
}
</script>

<style>
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
}

html {
  box-sizing: border-box;
}

body {
  background-color: #e5e5e5;
  height: 100vh;
  width: 100vw;
  font-family: sans-serif;
}
</style>
