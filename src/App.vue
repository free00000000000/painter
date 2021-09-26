<template>
  <Tool
    @ColorChange = 'Color = $event'
    @RowChange = 'Row = parseInt($event)'
    @ColChange = 'Col = parseInt($event)'
    @Clean = '$refs.Canvas.cleanCanvas()'
    @Save = '$refs.Canvas.saveCanvas()'
    @ModeChange = 'modeChange($event)'
  />
  <Canvas 
    ref = 'Canvas'
    :Color = 'Color'
    :Row = 'Row'
    :Col = 'Col'
    :isMoving = 'isMoving'
    :Mode = 'Mode'
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
      isMoving: false,
      Mode: 'Draw'
    }
  },
  methods: {
    modeChange(val){
      if(this.Mode === 'Draw'){
        this.Mode = val
      }else if(this.Mode === 'Move'){
        this.Mode = 'Draw'
      }
    },
  },
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
