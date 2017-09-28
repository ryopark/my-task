<template lang='pug'>
.hint
  .control-area
    .knob(:style='knobStyle')
</template>

<script>
import $ from 'jquery'
import 'jquery-ui'

export default {
  name: 'Hint',
  props: ['hint'],
  computed: {
    position: {
      get () {
        const array = this.hint.split(',')
        return {
          x: parseInt(array[0]),
          y: parseInt(array[1])
        }
      },
      set (val) {
        this.$emit('update', `${val.x},${val.y}`)
      }
    },
    knobStyle () {
      return {
        top: `${this.position.y}px`,
        left: `${this.position.x}px`
      }
    }
  },
  mounted () {
    $('.hint > .control-area > .knob').draggable({
      start: () => {
      },
      drag: (e, ui) => {
        this.position = {
          x: ui.position.left,
          y: ui.position.top
        }
      },
      stop: () => {
      }
    })

    
  }
}
</script>

<style lang='stylus' scoped>
.hint
  width 300px
  height 300px
  background #fff
  margin-top 10px
  display flex
  align-items center
  align-contetn center
  justify-content center
  h1
    font-weight normal
    margin 0
  .control-area
    width 100px
    height 100px
    position relative
    .knob
      position absolute
      width 16px
      height 16px
      margin-left -8px
      margin-top -8px
      background #A623B8
      border-radius 50%
      cursor move
      box-shadow 0 0 0 0 rgba(166, 35, 184, 0.0)
      transition box-shadow 0.2s cubic-bezier(0.4, 0.4, 0, 1)
      &:hover
        box-shadow 0 0 0 4px rgba(166, 35, 184, 0.2)
</style>
