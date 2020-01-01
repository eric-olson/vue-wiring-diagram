<template>
  <div>
    <div class="info-header">
      <p v-if="selectedComponent == null">No component selected</p>
      <span v-if="selectedComponent">
        <p>Info for {{ selectedComponent.name }}:</p>
        <p>{{ selectedComponent.description }}</p>
      </span>
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
.info-header {
  text-align: center;
}
</style>
