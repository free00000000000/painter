<template>
  <Tool
    @ColorChange = 'Color = $event'
    @RowChange = 'Row = parseInt($event)'
    @ColChange = 'Col = parseInt($event)'
    @Clean = '$refs.Canvas.cleanCanvas()'
    @Save = '$refs.Canvas.saveCanvas()'
  />
  <Canvas 
    ref = 'Canvas'
    :Color = 'Color'
    :Row = 'Row'
    :Col = 'Col'
  />
</template>

<script>
import Canvas from './components/Canvas/canvas.vue'
import Tool from './components/Tool/tool.vue'

export default {
  name: 'App',
  components: {
    Canvas,
    Tool,
  },
  data: function(){
    return {
      Color: 'red',
      Row: 20,
      Col: 20,
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
