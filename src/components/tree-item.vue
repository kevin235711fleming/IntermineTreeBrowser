<template>
    <li>
        <div
            @click="toggle">
            {{ item.displayName }}
            <span v-if="item.isTreeNode">
                [{{ showChildren ? '-' : '+' }}]
            </span>
        </div>
    <ul v-show="showChildren || rootNode" v-if="item.isTreeNode || rootNode">
        <tree-item
         v-for="child in item"
         :key="child.name"
         :item="child"
         :rootNode=false>
        </tree-item>
    </ul>
  </li>
</template>

<script>
export default {
    name: 'tree-item',
    props: {
        item: Object,
        rootNode: Boolean
    },
    data: function() {
        return {
          showChildren: false
        }
    },
    methods: {
        toggle: function() {
          if (this.isNode) {
            this.showChildren = !this.showChildren;
          }
        }
    }
}

</script>