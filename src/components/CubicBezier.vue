<template lang='pug'>
.cubic-bezier
  .control-area
    .p1(:style='p1Style')
    .p2(:style='p2Style')
    .p3(:style='p3Style')
    .p4(:style='p4Style')
    svg
      path(:style='lineStyle')
      path(:style='p1_lineStyle')
      path(:style='p2_lineStyle')
      path(:style='cubicStyle')
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
          p1_x: parseInt(array[0]),
          p1_y: parseInt(array[1]),
          p2_x: parseInt(array[2]),
          p2_y: parseInt(array[3])
        }
      },
      set (val) {
        this.$emit('update', `cubic-bezier(${val.p1_x},${val.p1_y},${val.p2_x},${val.p2_y})`)
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
    p3Style () {
      return {
        top: '150px',
        left: '200px'
      }
    },
    p4Style () {
      return {
        top: '350px',
        left: '0px'
      }
    },
    p1_lineStyle () {
      return {
        stroke: 'black',
        fill: 'none',
        d: `path('M 0,350 L ${this.position.p1_x}  ${this.position.p1_y}')`
      }
    },
    lineStyle () {
      return {
        stroke: 'black',
        fill: 'rgba(124,240,10,0.5)',
        d: "path('M 0,350 L 350, 0')"
      }
    },
    p2_lineStyle () {
      return {
        stroke: 'black',
        fill: 'none',
        d: `path('M 200 150 L ${this.position.p2_x} ${this.position.p2_y}')`
      }
    },
    cubicStyle () {
      return {
        strokeWidth: '5px',
        stroke: 'purple',
        fill: 'none',
        d: `path('M 200, 150 C ${this.position.p2_x} ${this.position.p2_y}, ${this.position.p1_x} ${this.position.p1_y}, 0 350 ')`
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
    width 200px
    height 500px
    position relative
    svg
      width 100%
      height 100%
    .p1,.p2
      position absolute
      width 16px
      height 16px
      background #A623B8
      border-radius 50%
      cursor move
      margin-top -8px
      margin-left -8px 
      box-shadow 0 0 0 0 rgba(166, 35, 184, 0.0)
      transition box-shadow 0.2s cubic-bezier(0.4, 0.4, 0, 1)
      &:hover
        box-shadow 0 0 0 4px rgba(166, 35, 184, 0.2)
    .p3,.p4
      position absolute
      width 16px
      height 16px
      background #56365b
      border-radius 50%
      cursor move
      margin-top -8px
      margin-left -8px
      box-shadow 0 0 0 0 rgba(166, 35, 184, 0.0)
      transition box-shadow 0.2s cubic-bezier(0.4, 0.4, 0, 1)
      &:hover
        box-shadow 0 0 0 4px rgba(166, 35, 184, 0.2)
    
</style>
