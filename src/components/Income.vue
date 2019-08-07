<template>
    <div>
        <b-container fluid>
            <b-row class="justify-content-md-center">
                <div class="col-1">
                    <button @click="add" class="btn btn-secondary button">Add</button>
                </div>

                <div class="col-7">
                    <h3>Income {{ draggingInfo }}</h3>

                    <draggable :list="list" class="list-group" handle=".handle" tag="ul">
                        <li
                                :key="element.name"
                                class="list-group-item"
                                v-for="(element, idx) in list"
                        >
                            <i class="fa fa-align-justify handle"></i>

                            <span class="text">{{ element.name }} </span>

                            <input class="form-control" type="text" v-model="element.text"/>

                            <i @click="removeAt(idx)" class="fa fa-times close"></i>
                        </li>
                    </draggable>
                </div>

                <rawDisplayer :value="list" class="col-3" title="List"/>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    let id = 3;
    import draggable from "vuedraggable";

    export default {
        name: "Income",
        display: "Handle",
        instruction: "Drag using the handle icon",
        order: 5,
        components: {
            draggable
        },
        data() {
            return {
                list: [
                    {name: "John", text: "", id: 0},
                    {name: "Joao", text: "", id: 1},
                    {name: "Jean", text: "", id: 2}
                ],
                dragging: false
            };
        },
        computed: {
            draggingInfo() {
                return this.dragging ? "under drag" : "";
            }
        },
        methods: {
            removeAt(idx) {
                this.list.splice(idx, 1);
            },
            add: function () {
                id++;
                this.list.push({name: "Juan " + id, id, text: ""});
            }
        }
    };
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