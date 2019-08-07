<template>
    <div>
        <draggable v-model="myArray" group="people" @start="drag=true" @end="drag=false">
            <div v-for="element in myArray" :key="element.id">
                {{element.name}}
            </div>
        </draggable>
    </div>
</template>

<script>
    import draggable from 'vuedraggable'

    export default {
        name: "DragPriorities",
        props: {
            categories: {
                default: () => ['Daily Spending', 'Taxes', 'Flat', 'Savings'],
                type: Array,
            },
        },
        data() {
            return {
                myArray: this.categories.map((name, index) => {
                    return {name, order: index + 1, fixed: false};
                }),
            };
        },
        components: {
            draggable,
        },
    }
</script>

<style scoped>

</style>