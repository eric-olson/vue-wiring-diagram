<template>
  <g :transform="'translate(' + x + ',' + y + ')'">
    <rect class="wire-node-target" width="10px" height="10px" rx="2px" />
    <text
      class="wire-node-name"
      :class="flip ? 'node-flip' : 'node-normal'"
      :x="textPos.x"
      :y="textPos.y"
    >{{ name }}</text>
  </g>
</template>

<script>
export default {
  name: 'WireNode',
  props: {
    x: Number,
    y: Number,
    name: String,
    flip: Boolean,
  },
  computed: {
    textPos() {
      const textX = this.flip ? '-5px' : '15px'
      const textY = '-2px'
      return {
        x: textX,
        y: textY,
      }
    },
  },
}
</script>

<style>
.wire-node-target {
  stroke: black;
  fill: white;
}
.wire-node-name {
  fill: white;
  font-size: 0.8em;
  dominant-baseline: hanging;
}
.node-normal {
  text-anchor: start;
}
.node-flip {
  text-anchor: end;
}
</style>
