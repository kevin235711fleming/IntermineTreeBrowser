<template>
  <SourceSelector @update:source="source = $event"/>
  <TreeView :source="source"/>
</template>

<script>
import TreeView from './components/tree-view.vue'
import SourceSelector from './components/source-selector.vue'

export default {
  name: 'App',
  components: {
    TreeView,
    SourceSelector
  },
  data () {
    return {
      source: 'https://www.flymine.org/flymine/service/model?format=json'
    }
  }
}
</script>

<style>
body {
  margin: 30px;
}
</style>
