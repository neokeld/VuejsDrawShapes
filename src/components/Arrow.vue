<template>
  <fragment>
  <circle name="start-circle" :cx="arrow.sx" :cy="arrow.sy" r="4" />
  <path name="line-between" :d="'M' + arrow.sx + ',' + arrow.sy + ' Q' + arrow.cx +',' + arrow.cy + ' ' + arrow.ex + ',' + arrow.ey" fill="none" />
  <polygon
    name="end-arrow"
    points="0,-6 12,0, 0,6"
    :transform="'translate(' + arrow.ex + ',' + arrow.ey +') rotate(' + endAngleAsDegrees + ')'"
  />
  </fragment>
</template>

<script>
import { getBoxToBoxArrow } from 'perfect-arrows'
import { Fragment } from 'vue-fragment'

const buildArrow = (p1, p2) => {
  const bow = 0.25
  const stretch = 0.5
  const padStart = 0
  const padEnd = 16
  const stretchMin = 0
  const stretchMax = 360
  const flip = false
  const straights = true

  const [sx, sy, cx, cy, ex, ey, ae, as, ec] = getBoxToBoxArrow(
    p1.x,
    p1.y,
    p1.w,
    p1.h,
    p2.x,
    p2.y,
    p2.w,
    p2.h,
    {
      padStart,
      padEnd,
      bow,
      straights,
      stretch,
      stretchMax,
      stretchMin,
      flip
    }
  )

  const arrow = {sx, sy, cx, cy, ex, ey, ae, as, ec}
  const endAngleAsDegrees = arrow.ae * (180 / Math.PI)

  return {
    arrow,
    endAngleAsDegrees
  }
}

export default {
  name: 'Arrow',
  components: { Fragment },
  data () {
    return buildArrow(this.p1, this.p2)
  },
  props: {
    p1: Object,
    p2: Object
  },
  watch: {
    p1: {
      handler: function (val, oldVal) {
        const { arrow, endAngleAsDegrees } = buildArrow(val, this.p2)
        this.arrow = arrow
        this.endAngleAsDegrees = endAngleAsDegrees
      },
      deep: true
    },
    p2: {
      handler: function (val, oldVal) {
        const { arrow, endAngleAsDegrees } = buildArrow(this.p1, val)
        this.arrow = arrow
        this.endAngleAsDegrees = endAngleAsDegrees
      },
      deep: true
    }
  }
}
</script>
