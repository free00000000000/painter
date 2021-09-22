<script>
export default {
  name: 'Canvas',
  props: ['Color'],
  data: function(){
    return {
      Canvas: null,
      Row: 20,
      Col: 20,
      Size: 20, // each block length of side
      isDrawing: false,
    }
  },
  mounted() {
    this.Canvas = this.$refs['drawCanvas']
    // set size
    this.setCanvasSize(this.Col*this.Size+1, this.Row*this.Size+1) // +1: bias for width of line
    // draw
    this.drawGrid(this.Row, this.Col, this.Size, 'lightgrey')
  },
  methods:{
    setCanvasSize(width, height){
      this.Canvas.width = width
      this.Canvas.height = height
    },
    getClickLocation(event){
      let rect = this.Canvas.getBoundingClientRect();
      let x = event.clientX - rect.left;
      let y = event.clientY - rect.top;
      return {x, y}
    },
    getBlockByLocation(x, y){
      let x_idx = Math.floor(x / this.Col)
      let y_idx = Math.floor(y / this.Row)
      return {x_idx, y_idx}
    },
    mouseDown(event){
      this.isDrawing = true
      let pos = this.getClickLocation(event)
      let blk = this.getBlockByLocation(pos.x, pos.y)
      this.drawRectangle(
        {x: blk.x_idx * this.Size, y: blk.y_idx * this.Size},
        {x: (blk.x_idx+1) * this.Size, y: (blk.y_idx+1) * this.Size}, this.Color, false)
      this.drawGrid(this.Row, this.Col, this.Size, 'lightgrey')
    },
    mouseMove(event){
      if(this.isDrawing){
        let pos = this.getClickLocation(event)
        let blk = this.getBlockByLocation(pos.x, pos.y)
        this.drawRectangle(
          {x: blk.x_idx * this.Size, y: blk.y_idx * this.Size},
          {x: (blk.x_idx+1) * this.Size, y: (blk.y_idx+1) * this.Size}, this.Color, false)
        this.drawGrid(this.Row, this.Col, this.Size, 'lightgrey')
      }
    },
    mouseUp(){
      this.isDrawing = false
    },
    drawGrid(row, col, size, color){
      // draw grid
      for(var x = 0; x <= col; ++x){
        this.drawLine(
          {x: x*size + 0.5, y: 0}, 
          {x: x*size + 0.5, y: row*size}, color)
      }
      for(var y = 0; y <= row; ++y){
        this.drawLine(
          {x: 0, y: y*size + 0.5}, 
          {x: col*size, y: y*size + 0.5}, color)
      }
    },
    drawLine(from, to, color = null){
      // get context
      let ctx = this.Canvas.getContext('2d')
      
      // setting
      if(color != null){
        ctx.strokeStyle = color
      }

      ctx.beginPath();  // Start a new path
      ctx.moveTo(from.x, from.y);
      ctx.lineTo(to.x, to.y);
      ctx.stroke();  // Render the path
    },
    drawRectangle(from, to, color = null, isUnder = false){
      // get context
      let ctx = this.Canvas.getContext('2d')

      // setting
      if(color != null){
        ctx.fillStyle = color
      }
      if(isUnder){
        ctx.globalCompositeOperation = 'destination-over'
      }

      ctx.beginPath();
      ctx.rect(from.x, from.y, to.x - from.x, to.y - from.y);
      ctx.fill();
    }
  }
}
</script>

<template src="./canvas.html"></template>
<style src="./canvas.css" scoped></style>