<template lang='pug'>
.cubic-bezier
  .control-area
    .p1(:style='p1Style')
    .p2(:style='p2Style')
    svg(viewBox='0 0 200 600')
      path.line
      path.cubicPath(:style='cubicStyle')
      path.toP1(:style='toP1Style')
      path.toP2(:style='toP2Style')
</template>

<script>
import $ from 'jquery'
import 'jquery-ui'

export default {
  name: 'CubicBezier',
  props: ['cubicBezier'],
  computed: {
    position: {
      get () {
        const cubicBezier = String(this.cubicBezier)
        const start = cubicBezier.indexOf('(') + 1
        const end =  cubicBezier.indexOf(')') 
        const array = cubicBezier.slice(start, end).split(',')
        return {
          p1_x: parseFloat(array[0])*200,
          p1_y: (400 - parseFloat(array[1])*200),
          p2_x: parseFloat(array[2])*200,
          p2_y: (400 - parseFloat(array[3])*200)
        }
      },
      set (val) {
        this.$emit('update', `cubic-bezier(${Math.round(val.p1_x/2)/100}, ${Math.round((400 - val.p1_y)/2)/100}, ${Math.round(val.p2_x/2)/100}, ${Math.round((400 - val.p2_y)/2)/100})`)
      }

    },
    p1Style () {
      return {
        top: `${this.position.p1_y}px`,
        left: `${this.position.p1_x}px`
      }
    },
    p2Style () {
      return {
        top: `${this.position.p2_y}px`,
        left: `${this.position.p2_x}px`
      }
    },
    toP1Style () {
      return {
        d: `path('M 0,400 L ${this.position.p1_x}  ${this.position.p1_y}')`
      }
    },
    toP2Style () {
      return {
        d: `path('M 200 200 L ${this.position.p2_x} ${this.position.p2_y}')`
      }
    },
    cubicStyle () {
      return {
        d: `path('M 200, 200 C ${this.position.p2_x} ${this.position.p2_y}, ${this.position.p1_x} ${this.position.p1_y}, 0 400 ')`
      }
    }  
  },
  mounted () {
    $('.cubic-bezier > .control-area > .p1').draggable({
      containment: 'parent',
      start: () => {
      },
      drag: (e, ui) => {
        this.position = {
          p1_x: ui.position.left,
          p1_y: ui.position.top,
          p2_x: this.position.p2_x,
          p2_y: this.position.p2_y,
        }
      },
      stop: () => {
      }
    }),
    $('.cubic-bezier > .control-area > .p2').draggable({
      containment: 'parent',
      start: () => {
      },
      drag: (e, ui) => {
        this.position = {
          p2_x: ui.position.left,
          p2_y: ui.position.top,
          p1_x: this.position.p1_x,
          p1_y: this.position.p1_y,
        }
      },
      stop: () => {
      }
    })    
  }
}
</script>

<style lang='stylus' scoped>
.cubic-bezier
  width 300px
  height 300px
  background #fff
  margin-top 10px
  display flex
  align-items center
  align-contetn center
  justify-content center
  .control-area
    width 210px
    height 600px
    position relative
    .p1,.p2
      position absolute
      width 16px
      height 16px
      background #A623B8
      border-radius 50%
      margin-top -8px
      margin-left -3px 
      cursor move
      box-shadow 0 0 0 0 rgba(166, 35, 184, 0.0)
      transition box-shadow 0.2s cubic-bezier(0.4, 0.4, 0, 1)
      &:hover
        box-shadow 0 0 0 4px rgba(166, 35, 184, 0.2)
    svg
      width 210px
      height 600px
      .line
        stroke #000000
        stroke-width 2px
        fill: rgba(124,240,10,0.5)
        d: path('M 0,400 L 200, 200')
        stroke-opacity: 0.104761096
        stroke-linecap round
        stroke-linejoin round
      .toP1,.toP2
        stroke-width 2px
        stroke #A623B8
        fill none
        stroke-linecap round
        stroke-linejoin round
      .cubicPath
        stroke-width 4px
        stroke-linecap round
        stroke-linejoin round
        stroke #000000
        fill none




</style>
