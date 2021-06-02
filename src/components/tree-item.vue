<template>
    <li v-show="item.name!='id'">
        <div
            @click="toggle">
            {{ item.displayName }}
            <span v-if="item.isTreeNode">
                [{{ showChildren ? '-' : '+' }}]
            </span>
        </div>
    <ul v-show="showChildren || root" v-if="showNode() || root">
        <tree-item
         v-for="child in makeSortedTree"
         :key="child.name"
         :item="child"
         :treeData="treeData"
         :root=false>
        </tree-item>
    </ul>
  </li>
</template>

<script>
export default {
    name: 'tree-item',
    props: {
        item: Object,
        treeData: Object,
        root: Boolean
    },
    data: function() {
        return {
          showChildren: false
        }
    },
    methods: {
        toggle: function() {
          if (this.item.isTreeNode) {
            this.showChildren = !this.showChildren;
          }
        },
        showNode: function() {
            return this.item.isTreeNode && this.showChildren
        }
    },
    computed: {
        sortedClassAttributes () {
          return Object.fromEntries(Object.entries(this.item.attributes).sort())
        },
        makeSortedTree () {
            let result = []
              let leafAttributes = (this.root) ? Object.fromEntries(Object.entries(this.item.attributes).sort()) 
                                            : Object.fromEntries(Object.entries(this.treeData[this.item.referencedType].attributes).sort())
              let classNodes = (this.root) ? Object.fromEntries(Object.entries({...this.item.collections, ...this.item.references}).sort()) 
                                            : Object.fromEntries(Object.entries({...this.treeData[this.item.referencedType].collections, ...this.treeData[this.item.referencedType].references}).sort())
              Object.keys(leafAttributes).forEach(objectKey => result.push(leafAttributes[objectKey]))
              Object.keys(classNodes).forEach(objectKey => {
                classNodes[objectKey].isTreeNode = true
                result.push(classNodes[objectKey])
              })
              return result
        }
  }
}

</script>