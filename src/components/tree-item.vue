<template>
    <li v-show="item.name!='id'"
        :class="{ attribute: !item.isTreeNode, expanded: showChildren }">
        <div
            @click="toggle">
            {{ item.displayName }}
            <span v-if="item.isTreeNode">
                [{{ showChildren ? '-' : '+' }}]
            </span>
        </div>
        <transition name="fade" mode="out-in" appear>
            <ul v-show="showChildren || root" v-if="showNode() || root">
                <tree-item
                 v-for="child in makeSortedTree"
                 :key="child.name"
                 :item="child"
                 :treeData="treeData"
                 :root=false>
                </tree-item>
            </ul>
        </transition>
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

<style scoped>
ul { margin-top: 0em }
ul li { margin-bottom: 0em }
li div {
    display: inline-block;
}
li:not(.attribute)>div {
    color: #9b4dca;
    cursor: zoom-in;
}
li.expanded>div {
    cursor: zoom-out;
}
ul ul { 
    font-size: 98%;
    margin-bottom: .5rem;
}
.attribute { list-style: square inside; }

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>