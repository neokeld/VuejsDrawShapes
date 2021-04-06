<template>
    <rect
        :x="x"
        :y="y"
        :width="w"
        :height="h"
        fill="white"
        fill-opacity="0.1"
        stroke="black"
    />
</template>

<script>
export default {
  name: 'RectShape',
  props: {
    x: Number,
    y: Number,
    w: Number,
    h: Number
  }
}
</script>
