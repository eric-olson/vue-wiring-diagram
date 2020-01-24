<template>
  <div id="app">
    <ComponentPicker
      class="picker"
      :components="componentTypes"
      @select-component="(sel) => (selectedComponent = sel)"
    />
    <WiringDiagram
      class="wiring-diagram"
      :components="components"
      :selected-component="selectedComponent"
      @select-component="(sel) => (selectedComponent = sel)"
    />
    <InfoPane class="info-pane" :selected-component="selectedComponent" />
  </div>
</template>

<script>
import ComponentPicker from './components/ComponentPicker.vue'
import InfoPane from './components/InfoPane.vue'
import WiringDiagram from './components/WiringDiagram.vue'
import initComponents from './assets/components.json'

export default {
  name: 'app',
  components: {
    ComponentPicker,
    InfoPane,
    WiringDiagram,
  },
  data() {
    return {
      selectedComponent: null,
      components: initComponents.currentState,
      componentTypes: initComponents.componentTypes,
    }
  },
}
</script>

<style>
html,
body {
  height: 100%;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
  height: 100%;
  display: flex;
  flex-flow: row wrap;
  justify-content: space-around;
}
.picker {
  flex: 1 0px;
  margin: auto;
  height: 100%;
}
.wiring-diagram {
  flex: 3 0px;
  margin: auto;
  border: 1px solid grey;
}
.info-pane {
  flex: 1 0px;
  margin: auto;
  height: 100%;
}
</style>
