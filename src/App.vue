<template>
  <div id="app">
    <WiringDiagram class="wiring-diagram" @select-component="(sel) => (selectedComponent = sel)" />
    <InfoPane class="info-pane" :selected-component="selectedComponent" />
  </div>
</template>

<script>
import WiringDiagram from './components/WiringDiagram.vue'
import InfoPane from './components/InfoPane.vue'

export default {
  name: 'app',
  components: {
    WiringDiagram,
    InfoPane,
  },
  data() {
    return {
      selectedComponent: null,
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
