<script>
import { ColorPicker } from 'vue-color-kit'
import 'vue-color-kit/dist/vue-color-kit.css'
import SvgIcon from '@jamescoyle/vue-icon'
import { mdiContentSave } from '@mdi/js';
import { mdiDelete } from '@mdi/js';
import { mdiPalette } from '@mdi/js';

export default {
  name: 'Tool',
  components: {
		SvgIcon,
    ColorPicker,
	},
  data: function(){
    return {
      color: '#FF0000', // current color
      suckerCanvas: null,
      suckerArea: [],
      isSucking: false,
      hide: true,

      currentcolor: {
        'background-color': 'red'
      },
      
      mid_save: mdiContentSave,
      mid_clean: mdiDelete,
      mdi_palette: mdiPalette,
    }
  },
  methods: {
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
    changeColor(color) {
        const { r, g, b, a } = color.rgba
        this.color = `rgba(${r}, ${g}, ${b}, ${a})`
        this.$emit('ColorChange', this.color)
        this.currentcolor['background-color'] = this.color
      },
      openSucker(isOpen) {
        // https://vuejsexamples.com/a-lightweight-color-picker-for-vue-3/
        if (isOpen) {
          // ... canvas be created
          // this.suckerCanvas = canvas
          // this.suckerArea = [x1, y1, x2, y2]
        } else {
          // this.suckerCanvas && this.suckerCanvas.remove
        }
      },
  }
}
</script>

<template src="./tool.html"></template>
<style src="./tool.css" scoped></style>