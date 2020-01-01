<template>
  <path class="node-link" :d="d" />
</template>

<script>
import * as d3 from 'd3'
export default {
  name: 'NodeLink',
  props: {
    source: Object,
    target: Object,
  },
  data() {
    return {
      pathGenerator: d3
        .linkHorizontal()
        .x((d) => d.x)
        .y((d) => d.y),
    }
  },
  computed: {
    d() {
      return this.pathGenerator({
        source: this.source,
        target: this.target,
      })
    },
  },
}
</script>

<style scoped>
.node-link {
  stroke: blue;
  stroke-width: 2px;
}
</style>
