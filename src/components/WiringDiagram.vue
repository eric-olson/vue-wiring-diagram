<template>
  <svg @click.self="deselect">
    <!-- Component Boxes -->
    <g class="box-group">
      <ComponentBubble
        v-for="(component, name) in components"
        :key="name"
        :name="name"
        :component="component"
        :is-selected="selected == name"
        @select-me="select(component, name)"
        @update-pos="
          (newPos, nodePos) => {
            component.x = newPos.x
            component.y = newPos.y
            updateNodePositions(nodePos)
          }
        "
      />
    </g>

    <!-- I/O links -->
    <g class="link-group">
      <NodeLink
        v-for="(connection, index) in connections"
        :key="index"
        :source="connection.source"
        :target="connection.target"
      />
      <NodeLink v-if="linkBeingCreated" :source="{ x: 0, y: 0 }" :target="{ x: 100, y: 100 }" />
    </g>
  </svg>
</template>

<script>
// import * as d3 from 'd3'
import initComponents from '../assets/components.json'
import ComponentBubble from './ComponentBubble.vue'
import NodeLink from './NodeLink.vue'

export default {
  name: 'WiringDiagram',
  components: {
    ComponentBubble,
    NodeLink,
  },
  data() {
    return {
      selected: '',
      components: initComponents,
      nodePositions: {},
      connectionDescriptions: [
        {
          source: 'component1_output_output1',
          target: 'component2_input_input1',
        },
      ],
      linkBeingCreated: null,
    }
  },
  computed: {
    connections() {
      const initCoords = {
        x: 0,
        y: 0,
      }
      const conns = {}
      this.connectionDescriptions.forEach((conn) => {
        conns[conn] = {
          source: this.nodePositions[conn.source] || initCoords,
          target: this.nodePositions[conn.target] || initCoords,
        }
      })
      return conns
    },
  },
  methods: {
    select(component, key) {
      this.$emit('select-component', component)
      this.selected = key
    },
    deselect() {
      this.$emit('select-component', null)
      this.selected = ''
    },
    updateNodePositions(newPositions) {
      this.nodePositions = { ...this.nodePositions, ...newPositions }
    },
  },
  mounted() {},
}
</script>

<style scoped>
svg {
  width: 100%;
  height: 100%;
}
path {
  fill: none;
  stroke: #76bf8a;
  stroke-width: 3px;
}
</style>
