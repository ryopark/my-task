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
    points: {
      get () {
        const string = this.cubicBezier.match(/cubic-bezier\((.*)\)/)[1]
        return string.split(',').map(v => Number(v))
      },
      set (val) {
        this.$emit('update', `cubic-bezier(${val[0]}, ${val[1]}, ${val[2]}, ${val[3]})`)
      }
    },
    p1: {
      get () {
        return {
          x: this.points[0] * 200,
          y: 400 - (this.points[1] * 200)
        }
      },
      set (val) {
        this.points[0] = Math.round(val.x / 2) / 100
        this.points[1] = Math.round((400 - val.y) / 2) / 100
        this.points = this.points
      }
    },
    p2: {
      get () {
        return {
          x: this.points[2] * 200,
          y: 400 - (this.points[3] * 200)
        }
      },
      set (val) {
        this.points[2] = Math.round(val.x / 2) / 100
        this.points[3] = Math.round((400 - val.y) / 2) / 100
        this.points = this.points
      }
    },
    p1Style () {
      return {
        top: `${this.p1.y}px`,
        left: `${this.p1.x}px`
      }
    },
    p2Style () {
      return {
        top: `${this.p2.y}px`,
        left: `${this.p2.x}px`
      }
    },
    toP1Style () {
      return {
        d: `path('M 0,400 L ${this.p1.x}  ${this.p1.y}')`
      }
    },
    toP2Style () {
      return {
        d: `path('M 200 200 L ${this.p2.x} ${this.p2.y}')`
      }
    },
    cubicStyle () {
      return {
        d: `path('M 200, 200 C ${this.p2.x} ${this.p2.y}, ${this.p1.x} ${this.p1.y}, 0 400 ')`
      }
    }
  },
  mounted () {
    $('.cubic-bezier > .control-area > .p1').draggable({
      containment: 'parent',
      drag: (e, ui) => {
        this.p1 = {
          x: ui.position.left,
          y: ui.position.top
        }
      }
    }),
    $('.cubic-bezier > .control-area > .p2').draggable({
      containment: 'parent',
      drag: (e, ui) => {
        this.p2 = {
          x: ui.position.left,
          y: ui.position.top
        }
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
    width 200px
    height 600px
    position relative
    .p1
    .p2
      position absolute
      width 16px
      height 16px
      background #A623B8
      border-radius 50%
      margin-top -8px
      margin-left -8px 
      cursor move
      box-shadow 0 0 0 0 rgba(166, 35, 184, 0.0)
      transition box-shadow 0.2s cubic-bezier(0.4, 0.4, 0, 1)
      &:hover
        box-shadow 0 0 0 4px rgba(166, 35, 184, 0.2)
    svg
      width 200px
      height 600px
      overflow visible
      .line
        stroke #000000
        stroke-width 2px
        fill rgba(124,240,10,0.5)
        d path('M 0,400 L 200, 200')
        stroke-opacity 0.104761096
        stroke-linecap round
        stroke-linejoin round
      .toP1
      .toP2
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
