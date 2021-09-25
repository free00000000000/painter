<script>
export default {
  name: 'Tool',
  data: function(){
    return {
      CurrentColor: 'red', // current color
    }
  },
  methods: {
    colorChange(event){
      this.$emit('ColorChange', event.target.value)
    },
    rowChange(event){
      this.$emit('RowChange', event.target.value)
    },
    colChange(event){
      this.$emit('ColChange', event.target.value)
    },
    clean(){
      this.$emit('Clean')
    },
    save(){
      this.$emit('Save')
    },
  }
}
</script>

<template src="./tool.html"></template>
<style src="./tool.css" scoped></style>