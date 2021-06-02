<template>
  <div>
    <select v-model="rootNode">
      <option v-for="classObject in sortedTreeClasses" 
              :key="classObject.name"
              >
              {{classObject.displayName}}
      </option>
    </select>
    <ul v-if="rootNode">
      <tree-item
        :item="makeTree(treeData[rootNode])" :rootNode=true></tree-item>
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
      rootNode: null,
      treeData: { attributes: {}}
    }
  },
  methods: {
    makeTree (root) {
      let result = []
      let leafAttributes = Object.fromEntries(Object.entries(root.attributes).sort())
      let classNodes = Object.fromEntries(Object.entries({...root.collections, ...root.references}).sort())
      Object.keys(leafAttributes).forEach(objectKey => result.push(leafAttributes[objectKey]))
      Object.keys(classNodes).forEach(objectKey => {
        classNodes[objectKey].showChildren = false
        classNodes[objectKey].isTreeNode = true
        result.push(classNodes[objectKey])
      })
      return result
    }
  },
  computed: {
    sortedTreeClasses () {
      return Object.fromEntries(Object.entries(this.treeData).sort())
    },
    sortedClassAttributes () {
      return Object.fromEntries(Object.entries(this.treeData[this.rootNode].attributes).sort())
    },
    isTreeNode: function() {
      return this.item.children && this.item.children.length;
    }
  },
  mounted () {
    fetch(this.source)
    .then(
      (response) => {
        if (response.status !== 200) {
          console.log('Looks like there was a problem. Status Code: ' +
            response.status);
          return;
        }

        // Examine the text in the response
        response.json().then((data) => {
          this.treeData = data.model.classes
        });
      }
    )
    .catch(function(err) {
      console.log('Fetch Error :-S', err);
    });

    }
}
</script>

<style scoped>

</style>
