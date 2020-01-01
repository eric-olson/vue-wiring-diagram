<template>
  <div>
    <div class="info-header">
      <div v-if="selectedComponent == null">
        <p v-if="selectedComponent == null">No component selected</p>
      </div>
      <div v-if="selectedComponent">
        <h2>{{ selectedComponent.name }}:</h2>
        <p>{{ selectedComponent.description }}</p>

        <h3>Inputs</h3>
        <li
          v-for="(input, name, idx) in selectedComponent.inputs"
          :key="'input' + idx"
        >{{ name }} ({{ input.type }})</li>

        <h3>Outputs</h3>
        <li
          v-for="(output, name, idx) in selectedComponent.outputs"
          :key="'output' + idx"
        >{{ name }} ({{ output.type }})</li>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'InfoPane',
  props: {
    selectedComponent: Object,
  },
}
</script>

<style scoped>
li {
  text-align: left;
  padding: 0 10%;
}
.info-header {
  text-align: center;
}
</style>
