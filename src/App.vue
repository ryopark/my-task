<template lang='pug'>
#app
  section
    h1 Cubic Bezier
    p {{cubicBezier}}
    CubicBezier(
      :cubicBezier='cubicBezier',
      @update='updateCubicBezier'
    )
  section
    h1 Hint
    p {{hint}}
    Hint(
      :hint='hint',
      @update='updateHint'
    )
</template>

<script>
import Hint from './components/Hint'
import CubicBezier from './components/CubicBezier'

export default {
  name: 'app',
  components: {
    Hint,
    CubicBezier
  },
  data () {
    return {
      hint: '20,20',
      cubicBezier: 'cubic-bezier(0,100,200,100)'
    }
  },
  methods: {
    updateHint (val) {
      this.hint = val
    },
    updateCubicBezier (val) {
      this.cubicBezier = val
    }
  }
}
</script>

<style lang='stylus'>
body
  margin 0
  background #f1f1f1
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 0px
  padding 50px
  display flex
  align-items center
  align-content center
  flex-wrap nowrap
  flex-direction column
  section
    margin-bottom 20px
*
  box-sizing border-box
  margin 0
  padding 0
</style>
