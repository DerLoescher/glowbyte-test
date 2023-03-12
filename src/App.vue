<template>
  <TheTable :tableData="tableData" />
</template>

<script>
import { result } from "./data/data.json";
import TheTable from './components/TheTable.vue'

export default {
  name: 'App',
  components: {
    TheTable
  },
  data() {
    return {
      tableData: result
    }
  }
}
</script>

<style>
html,
body {
  position: relative;
  height: 100%;
}

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

html {
  scroll-behavior: smooth;
}
</style>
