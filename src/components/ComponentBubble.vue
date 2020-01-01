<template>
  <g
    class="component-container"
    :transform="'translate(' + x + ',' + y + ')'"
    @dblclick="$emit('select-me')"
  >
    <rect
      class="outer-bubble"
      :class="{ moving: isMoving, selected: isSelected }"
      :width="width"
      :height="height"
      rx="15"
    />
    <text class="component-name" :x="width / 2" y="20">{{ name }}</text>
    <WireNode
      v-for="(input, wirename, index) in component.inputs"
      :key="name + '_input_' + wirename"
      :x="inputXOffset"
      :y="yOffset(index)"
      :name="wirename"
    />
    <WireNode
      v-for="(output, wirename, index) in component.outputs"
      :key="name + '_output_' + wirename"
      :x="outputXOffset"
      :y="yOffset(index)"
      :name="wirename"
      :flip="true"
    />
  </g>
</template>

<script>
import * as d3 from 'd3'
import WireNode from './WireNode.vue'
export default {
  name: 'ComponentBubble',
  components: {
    WireNode,
  },
  props: {
    component: {
      type: Object,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    isSelected: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      isMoving: false,
    }
  },
  computed: {
    x() {
      return this.component.x
    },
    y() {
      return this.component.y
    },
    width() {
      return 200
    },
    height() {
      return Math.max(100, Object.keys(this.component.inputs).length * 20 + 50)
    },
    inputXOffset() {
      return 10
    },
    outputXOffset() {
      return this.width - 20
    },
    nodePositions() {
      var positions = {}
      var idx = 0
      for (const input in this.component.inputs) {
        positions[this.name + '_input_' + input] = {
          x: this.x + this.inputXOffset + 5,
          y: this.y + this.yOffset(idx) + 5,
        }
        idx++
      }
      idx = 0
      for (const output in this.component.outputs) {
        positions[this.name + '_output_' + output] = {
          x: this.x + this.outputXOffset + 5,
          y: this.y + this.yOffset(idx) + 5,
        }
        idx++
      }
      return positions
    },
  },
  methods: {
    startMove() {
      this.isMoving = true
    },
    updateMove() {
      this.$emit(
        'update-pos',
        {
          x: this.x + d3.event.dx,
          y: this.y + d3.event.dy,
        },
        this.nodePositions
      )
    },
    stopMove() {
      this.isMoving = false
    },
    yOffset(index) {
      return index * 20 + 40
    },
  },
  mounted() {
    this.$emit(
      'update-pos',
      {
        x: this.x,
        y: this.y,
      },
      this.nodePositions
    )
    /** use d3 DOM manip to set up drag & drop magic */
    d3.select(this.$el).call(
      d3
        .drag()
        .on('start', this.startMove)
        .on('drag', this.updateMove)
        .on('end', this.stopMove)
    )
  },
}
</script>

<style scoped>
.outer-bubble {
  cursor: move;
  stroke: black;
  fill: #a56d6d;
}
.component-name {
  fill: black;
  text-anchor: middle;
  dominant-baseline: middle;
}
.moving {
  stroke-width: 3px;
}
.selected {
  stroke-width: 2px;
  stroke: red;
}
</style>
