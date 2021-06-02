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
        :item="treeData[rootNode]" :treeData=treeData :root=true></tree-item>
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
    
  },
  computed: {
    sortedTreeClasses () {
      return Object.fromEntries(Object.entries(this.treeData).sort())
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
