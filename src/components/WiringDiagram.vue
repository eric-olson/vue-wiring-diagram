<template>
  <svg @click.self="handleClick">
    <!-- Component Boxes -->
    <g class="box-group">
      <ComponentBubble
        v-for="(component, name) in components"
        :key="name"
        :name="name"
        :component="component"
        :x="component.x"
        :y="component.y"
        :is-selected="selected == name"
        @select-me="select(component, name)"
        @update-pos="
          (newPos, nodePos) => {
            component.x = newPos.x
            component.y = newPos.y
            updateNodePositions(nodePos)
          }
        "
        @new-wire="(nodeId) => newWire(nodeId)"
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
      <NodeLink
        v-if="linkBeingCreated"
        :source="nodePositions[linkBeingCreated]"
        :target="mousePos"
      />
    </g>
  </svg>
</template>

<script>
import * as d3 from 'd3'
import ComponentBubble from './ComponentBubble.vue'
import NodeLink from './NodeLink.vue'

export default {
  name: 'WiringDiagram',
  components: {
    ComponentBubble,
    NodeLink,
  },
  props: {
    components: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      selected: '',
      nodePositions: {},
      connectionDescriptions: [
        {
          source: 'component2_output_power',
          target: 'component1_input_power',
        },
        {
          source: 'component2_output_time',
          target: 'component1_input_time',
        },
      ],
      linkBeingCreated: null,
      mousePos: {
        x: 50,
        y: 50,
      },
    }
  },
  computed: {
    connections() {
      const initCoords = {
        x: 0,
        y: 0,
      }
      const conns = []
      this.connectionDescriptions.forEach((conn) => {
        conns.push({
          source: this.nodePositions[conn.source] || initCoords,
          target: this.nodePositions[conn.target] || initCoords,
        })
      })
      return conns
    },
  },
  methods: {
    select(component, key) {
      this.$emit('select-component', component)
      this.selected = key
    },
    handleClick() {
      // if link is being created, click on canvas means "cancel creating link"
      if (this.linkBeingCreated) {
        this.linkBeingCreated = null
        return
      }
      // otherwise, click on canvas means "deselect component"
      this.$emit('select-component', null)
      this.selected = ''
    },
    updateNodePositions(newPositions) {
      this.nodePositions = { ...this.nodePositions, ...newPositions }
    },
    newWire(nodeId) {
      if (this.linkBeingCreated === nodeId) {
        return
      }
      if (this.linkBeingCreated == null) {
        this.linkBeingCreated = nodeId
      } else {
        this.connectionDescriptions.push({
          source: this.linkBeingCreated,
          target: nodeId,
        })
        this.linkBeingCreated = null
      }
    },
  },
  mounted() {
    /** use d3 to track mousemove and update mousePos accordingly */
    d3.select(this.$el).on('mousemove', () => {
      this.mousePos = {
        x: d3.event.offsetX,
        y: d3.event.offsetY,
      }
    })
  },
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
.reset-text {
  color: white;
}
</style>
