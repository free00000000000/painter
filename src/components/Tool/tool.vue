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
      this.$emit('ColorChange', event.target.value);
    }
  }
}
</script>

<template src="./tool.html"></template>
<style src="./tool.css" scoped></style>