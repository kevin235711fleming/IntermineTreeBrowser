<template>
  <div>
    <select required
            v-model="rootNode" 
            id="classSelector">
      <option value="" disabled selected hidden>Please Choose...</option>
      <option v-for="classObject in sortedTreeClasses" 
              :key="classObject.name"
              :value="classObject.name"
              >
              {{classObject.displayName}}
      </option>
    </select>
    <ul v-if="rootNode" class="tree">
      <tree-item
        :item="treeData[rootNode]" 
        :treeData=treeData 
        :root=true>
        </tree-item>
    </ul>
    <p v-else>No root class selected.</p>
  </div>
</template>

<script>
import TreeItem from './tree-item.vue'
export default {
  name: 'tree-view',
  components: {
    TreeItem
  },
  props: {
    source: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      rootNode: false,
      treeData: { attributes: {}}
    }
  },
  methods: {
    fetchSource () {
      fetch(this.source)
      .then(
        (response) => {
          if (response.status !== 200) {
            console.log('Looks like there was a problem. Status Code: ' +
              response.status)
            return;
          }

          // Examine the text in the response
          response.json().then((data) => {
            this.treeData = data.model.classes
          });
        }
      )
      .catch(function(err) {
        console.log('Fetch Error :-S', err)
      })
      }
  },
  computed: {
    sortedTreeClasses () {
      return Object.fromEntries(Object.entries(this.treeData).sort())
    }
  },
  watch: {
    source (val) {
      if(val != '') this.fetchSource()
    }
  },
  mounted () {
    this.fetchSource()
  }
}
</script>

<style scoped>
#classSelector { width: 20em }
</style>
