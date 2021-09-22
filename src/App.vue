<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <Tool
    @ColorChange = 'Color = $event'
  />
  <Canvas 
    :Color = 'Color'
  />
</template>

<script>
import Canvas from './components/Canvas/canvas.vue'
import Tool from './components/Tool/tool.vue'

export default {
  name: 'App',
  components: {
    Canvas,
    Tool
  },
  data: function(){
    return {
      Color: 'red'
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
