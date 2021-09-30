<template>
  <Tool
    @ColorChange = 'Color = $event'
    @RowChange = 'Row = parseInt($event)'
    @ColChange = 'Col = parseInt($event)'
    @Clean = '$refs.Canvas.cleanCanvas()'
    @Save = '$refs.Canvas.saveCanvas()'
    @Undo = '$refs.Canvas.undoCanvas()'
    @Redo = '$refs.Canvas.redoCanvas()'
    @ModeChange = 'modeChange($event)'
  />
  <Canvas 
    ref = 'Canvas'
    :Color = 'Color'
    :BgColor = 'BgColor'
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
      BgColor: 'white',
      Row: 20,
      Col: 20,
      isMoving: false,
      Mode: 'Draw'
    }
  },
  methods: {
    modeChange(val){
      // FSM[From][Get] = To
      const mode_FSM = {
        'Draw': {
          'Draw': 'Draw',
          'Move': 'Move',
          'Fill': 'Fill',
          'Erase': 'Erase',
        },
        'Move': {
          'Draw': 'Draw',
          'Move': 'Draw',
          'Fill': 'Fill',
          'Erase': 'Erase',
        },
        'Fill': {
          'Draw': 'Draw',
          'Move': 'Move',
          'Fill': 'Draw',
          'Erase': 'Erase',
        },
        'Erase': {
          'Draw': 'Draw',
          'Move': 'Move',
          'Fill': 'Fill',
          'Erase': 'Draw',
        },
      }
      this.Mode = mode_FSM[this.Mode][val]
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
