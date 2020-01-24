<template>
  <svg>
    <ComponentBubble
      v-for="(component, name, idx) in components"
      :key="name"
      :name="name"
      :component="component"
      :x="10"
      :y="20 * idx + 40"
      :showIO="false"
      @select-me="$emit('select-component', component)"
    />
  </svg>
</template>

<script>
import ComponentBubble from './ComponentBubble.vue'

export default {
  name: 'ComponentPicker',
  components: {
    ComponentBubble,
  },
  props: {
    components: {
      type: Object,
      required: true,
    },
  },
}
</script>

<style></style>
