<template>
  <div id="app">
    <EditorScene />
  </div>
</template>

<script>
import EditorScene from './components/EditorScene.vue'

export default {
  name: 'App',
  components: {
    EditorScene
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
