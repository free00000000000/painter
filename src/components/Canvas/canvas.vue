<script>
export default {
  name: 'Canvas',
  props: ['Color', 'Row', 'Col', 'isMoving', 'Mode'],
  data: function(){
    return {
      Canvas: null,
      Size: 20, // each block length of side
      isPress: false,
      BackgroundColor: '#ffffff',
      GridColor: 'lightgrey',
      MoveOriginBlock: null,
    }
  },
  mounted() {
    this.Canvas = this.$refs.drawCanvas
    // set size
    this.setCanvasSize(this.Col*this.Size, this.Row*this.Size) 
    // draw
    this.drawGrid(this.Row, this.Col, this.Size, this.GridColor)
  },
  watch:{ 
    Row: function(newVal, oldVal){
      if(newVal != oldVal && !isNaN(newVal)){
        this.setCanvasSize(this.Col*this.Size, newVal*this.Size)
        this.drawGrid(newVal, this.Col, this.Size, this.GridColor)
      }
    },
    Col: function(newVal, oldVal){
      if(newVal != oldVal && !isNaN(newVal)){
        this.setCanvasSize(newVal*this.Size, this.Row*this.Size)
        this.drawGrid(this.Row, newVal, this.Size, this.GridColor)
      }
    }
  },
  methods:{
    setCanvasSize(width, height){
      this.Canvas.width = width+1 // +1: bias for width of line
      this.Canvas.height = height+1 // +1: bias for width of line
    },
    getClickLocation(event){
      let rect = this.Canvas.getBoundingClientRect();
      let x = event.clientX - rect.left;
      let y = event.clientY - rect.top;
      return {x, y}
    },
    getBlockByLocation(x, y){
      let x_idx = Math.floor(x / this.Size)
      let y_idx = Math.floor(y / this.Size)
      return {x_idx, y_idx}
    },
    mouseDown(event){
      event.preventDefault();
      this.isPress = true

      if(this.Mode === 'Draw'){
        // draw color
        let pos = this.getClickLocation(event)
        let blk = this.getBlockByLocation(pos.x, pos.y)
        this.drawRectangle(
          {x: blk.x_idx * this.Size, y: blk.y_idx * this.Size},
          {x: (blk.x_idx+1) * this.Size, y: (blk.y_idx+1) * this.Size}, this.Color, false)
        this.drawGrid(this.Row, this.Col, this.Size, this.GridColor)
      }else if(this.Mode === 'Move'){
        // record first block
        let pos = this.getClickLocation(event)
        let blk = this.getBlockByLocation(pos.x, pos.y)
        this.MoveOriginBlock = blk;
      }
    },
    mouseMove(event){
      event.preventDefault();
      if(this.isPress){
        if(this.Mode === 'Draw'){
          // draw color
          let pos = this.getClickLocation(event)
          let blk = this.getBlockByLocation(pos.x, pos.y)
          this.drawRectangle(
            {x: blk.x_idx * this.Size, y: blk.y_idx * this.Size},
            {x: (blk.x_idx+1) * this.Size, y: (blk.y_idx+1) * this.Size}, this.Color, false)
          this.drawGrid(this.Row, this.Col, this.Size, this.GridColor)
        }else if(this.Mode === 'Move'){
          // check move distance
          let pos = this.getClickLocation(event)
          let blk = this.getBlockByLocation(pos.x, pos.y)
          if(blk !== this.MoveOriginBlock){ // something changed
            let x_offset = blk.x_idx - this.MoveOriginBlock.x_idx
            let y_offset = blk.y_idx - this.MoveOriginBlock.y_idx
            let locate = {x: x_offset * this.Size, y: y_offset * this.Size}
            this.MoveCanvas(locate)
            // reset
            this.MoveOriginBlock = blk;
          }
        }
      }
    },
    mouseUp(){
      this.isPress = false
      this.MoveOriginBlock = null
    },
    mouseOut(){
      if(this.isPress){
        this.isPress = false
      }
    },
    MoveCanvas(locate){
      // get context
      let ctx = this.Canvas.getContext('2d')
      // get present canvas
      let image = ctx.getImageData(0, 0, this.Canvas.width, this.Canvas.height)
      // clean canvas
      this.cleanCanvas()
      // put canvas
      ctx.putImageData(image, locate.x, locate.y)
    },
    cleanCanvas(){
      this.drawRectangle({x: 0, y: 0}, {x: this.Canvas.width, y: this.Canvas.height}, this.BackgroundColor, false)
      // draw
      this.drawGrid(this.Row, this.Col, this.Size, this.GridColor)
    },
    saveCanvas(){
      // get image
      var image = this.Canvas.toDataURL("image/png").replace("image/png", "image/octet-stream");
      // download
      var a = document.createElement('a');
      a.href = image;
      a.download = 'Image.png';
      document.body.appendChild(a);
      a.click();
    },
    drawGrid(row, col, size, color){
      // draw grid
      for(var x = 0; x <= col; ++x){
        this.drawLine(
          {x: x*size + 0.5, y: 0.5}, 
          {x: x*size + 0.5, y: row*size + 0.5}, color)
      }
      for(var y = 0; y <= row; ++y){
        this.drawLine(
          {x: 0.5, y: y*size + 0.5}, 
          {x: col*size + 0.5, y: y*size + 0.5}, color)
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
      ctx.closePath();
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
      ctx.closePath();
      ctx.fill();
    }
  }
}
</script>

<template src="./canvas.html"></template>
<style src="./canvas.css" scoped></style>