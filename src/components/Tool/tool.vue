<script>
import SvgIcon from '@jamescoyle/vue-icon'
import { mdiContentSave } from '@mdi/js';
import { mdiDelete } from '@mdi/js';

export default {
  name: 'Tool',
  components: {
		SvgIcon
	},
  data: function(){
    return {
      CurrentColor: 'red', // current color
      mid_save: mdiContentSave,
      mid_clean: mdiDelete,
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