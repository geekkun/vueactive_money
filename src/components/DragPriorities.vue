<template>
    <div>
        <draggable v-model="myArray" group="people" @start="drag=true" @end="drag=false">
            <div v-for="element in myArray" :key="element.id">{{element.name}}</div>
        </draggable>
    </div>
</template>

<script>
    import draggable from 'vuedraggable'

    const message = [
        "vue.draggable",
        "draggable",
        "component",
        "for",
        "vue.js 2.0",
        "based",
        "on",
        "Sortablejs"
    ];
    export default {
        name: "DragPriorities",
        data() {
            return {
                myArray: message.map((name, index) => {
                    return {name, order: index + 1, fixed: false};
                }),
                list2: [],
                editable: true,
                isDragging: false,
                delayedDragging: false
            };
        },
        components: {
            draggable,
        },
    }
</script>

<style scoped>

</style>