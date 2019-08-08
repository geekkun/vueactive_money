<template>
    <div>
        <b-container fluid>
            <b-row class="justify-content-md-center">
                <h3>Priorities
                    <b-button @click="add" variant="outline-success">+</b-button>
                </h3>
            </b-row>
            <b-row class="justify-content-md-center">

                <draggable @end="drag=false" @start="drag=true" v-model="categories">

                    <li
                            :key="element.name"
                            class="list-group-item"
                            v-for="(element, idx) in categories"
                    >
                        <font-awesome-icon icon="align-justify"/>

                        <span class="text">{{ element.name }} </span>


                        <input class="form-control" type="text" v-model="element.text"/>
                        <span> {{element.text}}</span>
                        <!--                        <i @click="removeAt(idx)" class="fa fa-times close"></i>-->
                        <b-button @click="removeAt(idx)" variant="outline-success">-</b-button>

                    </li>

                </draggable>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    import draggable from 'vuedraggable'

    export default {
        name: "DragPriorities",
        props: {
            categories: {
                // default: () => ['Daily Spending', 'Taxes', 'Flat', 'Savings'],
                default: () => [
                    {name: "Taxes", text: "", id: 0},
                    {name: "Flat", text: "", id: 1},
                    {name: "Savings", text: "", id: 2},
                    {name: "Daily", text: "", id: 3}
                ],
                type: Array,
            },
        },
        data() {
            return {
                myArray: this.categories,
                id: this.categories.slice(-1)[0]['id'] + 1
            };
        },
        components: {
            draggable,
        },
        methods: {
            removeAt(idx) {
                this.myArray.splice(idx, 1);
            },
            add: function () {
                this.id++;
                this.myArray.push({name: "Juan " + this.id, text: "", id: this.id});
            }
        }
    }
</script>

<style scoped>

    .button {
        margin-top: 35px;
    }

    .handle {
        float: left;
        padding-top: 8px;
        padding-bottom: 8px;
    }

    .close {
        float: right;
        /*padding-top: 8px;*/
        /*padding-bottom: 8px;*/
    }

    input {
        display: inline-block;
        width: 50%;
    }

    .text {
        margin: 20px;
    }
</style>