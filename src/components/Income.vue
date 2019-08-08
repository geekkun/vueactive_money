<template>
    <div>
        <b-container fluid>
            <b-row class="justify-content-md-center">
                <h3>Income</h3>
            </b-row>
            <b-row class="justify-content-md-center">

                <draggable @end="drag=false" @start="drag=true" v-model="myArray">

                    <li
                            :key="element.name"
                            class="list-group-item"
                            v-for="(element, idx) in myArray"
                    >
                        <font-awesome-icon icon="align-justify"/>

                        <span class="text">{{ element.name }} </span>


                        <input class="form-control" type="text" v-model="element.text"/>
                        <span> {{element.text}}</span>
                        <!--                        <font-awesome-icon @click="removeAt(idx)" class="close"></font-awesome-icon>-->
                        <b-button @click="removeAt(idx)" variant="outline-success">-</b-button>

                    </li>
                    <br>

                </draggable>

            </b-row>
            <b-row class="justify-content-md-center">
                <!--                <li class="list-unstyled">-->
                <input class="form-control" placeholder="Category Name" type="text" v-model="category_name"/>
                <!--                        <font-awesome-icon icon="align-justify"/>-->
                <b-button @click="add" v-if="category_name" variant="outline-success">+</b-button>
                <!--                    </li>-->
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
                    {name: "Salary", text: "", id: 0},
                    {name: "Freelance 1", text: "", id: 1},
                    {name: "Freelance 2", text: "", id: 2},
                    {name: "Investments", text: "", id: 3}
                ],
                type: Array,
            },
        },
        data() {
            return {
                myArray: this.categories,
                id: this.categories.slice(-1)[0]['id'] + 1,
                category_name: null
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
                if (this.category_name) {
                    this.id++;
                    this.myArray.push({name: this.category_name, text: "", id: this.id});
                }
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
        text-align: center;
    }

    .text {
        margin: 20px;
    }
</style>