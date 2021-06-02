<template>
    <div>
        <input type="radio" id="flymine" @change="onChange($event)" value="https://www.flymine.org/flymine/service/model?format=json" v-model="source" />
        <label for="flymine">FlyMine</label>
        <input type="radio" id="humanmine" @change="onChange($event)" value="https://www.humanmine.org/humanmine/service/model\?format\=json" v-model="source" />
        <label for="humanmine">HumanMine</label>
    </div>
</template>

<script>
    export default {
        name: "source-selector",
        data () {
            return {
                source: 'https://www.flymine.org/flymine/service/model?format=json'
            }
        },
        methods: {
            onChange(event) {
                this.$emit('update:source', event.target.value)
            }
        }
    }
</script>

<style scoped>
#humanmine { margin-left: .5rem }
</style>