<template>
    <div>
        <b-button v-b-modal.income_card_settings>Launch demo modal</b-button>

        <b-modal id="income_card_settings" centered title="Income Settings">
            <b-container fluid>
                <form ref="form" @submit.stop.prevent="handleSubmit">
                    <b-form-group
                            :state="nameState"
                            label="Name"
                            label-for="name-input"
                            invalid-feedback="Name is required"
                    >
                        <b-form-input id="amount" :type="'number'" v-model="amount"></b-form-input>
                        <b-form-select v-model="name_selected" :options="name_options" class="mb-3">
                            <!-- This slot appears above the options from 'options' prop -->
                            <template v-slot:first>
                                <option :value="null" disabled>-- Please select an option --</option>
                            </template>

                            <!-- These options will appear after the ones from 'options' prop -->
                            <!--                            <option value="C">Option C</option>-->
                            <!--                            <option value="D">Option D</option>-->
                        </b-form-select>
                        <b-form-input
                                id="name-input"
                                v-model="name"
                                :state="nameState"
                                v-if="name_selected===false"
                        ></b-form-input>

                        <b-form-input :id="`type-date`" :type="'date'"></b-form-input>

                        <b-form-radio-group
                                id="paid-radio-group"
                                v-model="paid_selected"
                                :options="paid_options"
                                name="paid-radio-options"
                        ></b-form-radio-group>

                        <b-form-radio-group
                                id="radio-group-1"
                                v-model="tax_selected"
                                :options="tax_options"
                                name="radio-options"
                        ></b-form-radio-group>

                        <b-form-input
                                id="tax-input"
                                v-model="tax_input"
                                :state="tax_inputState"
                                v-if="tax_selected"
                        ></b-form-input>

                        <b-form-checkbox-group
                                id="checkbox-group-1"
                                v-model="selected"
                                :options="options"
                                name="flavour-1"
                        ></b-form-checkbox-group>


                        <b-row class="my-1" v-for="spend in selected" :key="spend">

                            <b-col sm="4">
                                <label :for="`type-${spend}`">{{ spend }}:</label>
                            </b-col>
                            <b-col sm="4">
                                <b-form-input :id="`type-${spend}`" :type="'number'"></b-form-input>
                            </b-col>
                            <b-col sm="4">
                                <b-form-radio-group
                                        :id="`radio-${spend}`"
                                        v-model="cur_v_prc_selected"
                                        :options="cur_v_prc_options"
                                        :name="`radio-${spend}-options`"
                                ></b-form-radio-group>
                            </b-col>
                        </b-row>

                    </b-form-group>
                </form>
            </b-container>
        </b-modal>
    </div>
</template>

<script>
    export default {
        name: "IncomeCard",
        data() {
            return {
                amount: 0,
                name_selected: null,
                name_options: [
                    {value: false, text: 'Other category'},
                ],

                paid_selected: false,
                paid_options: [
                    {text: 'Income is paid', value: true},
                    {text: 'Income is not yet paid', value: false},
                ],

                tax_selected: false,
                tax_options: [
                    {text: 'Income is taxed', value: true},
                    {text: 'Income is not taxed', value: false},
                ],


                tax_input: 0,
                selected: [], // Must be an array reference!
                options: [
                    {text: 'Flat', value: 'flat'},
                    {text: 'Monthly Subscribtions', value: 'subs'},
                    {text: 'Investment', value: 'invest'},
                    {text: 'Savings', value: 'save'},
                    {text: 'Daily Spendings', value: 'daily'},
                ],
                cur_v_prc_selected:[],
                cur_v_prc_options: [
                    {text: '%', value: true},
                    {text: '$', value: false},
                ],
            }
        }
    }
</script>

<style scoped>

</style>