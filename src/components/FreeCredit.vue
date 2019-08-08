<template>
    <div>
        <b-container fluid>
            <b-row class="justify-content-md-center">
                <h3>Credit</h3>
            </b-row>
            <b-row class="justify-content-md-center">
                <div>
                    <b-col>
                        <div class="price-holder">
                        <span>
                            {{days_left}} day<span v-if="days_left>1">s</span> to pay: {{current_month_debt | currency_rub}}
                        </span>
                            <span
                                    :class="difference >= 0 ? 'up' : 'down'"
                                    class="label ml-2">
                            {{difference_month | pct}}
                        </span>
                        </div>
                    </b-col>
                </div>
                <div>
                    <b-col>
                        <div class="price-holder">
                            <span>Your total debt is {{total_debt | currency_rub}}</span>
                            <span
                                    :class="difference >= 0 ? 'up' : 'down'"
                                    class="label ml-2">
                            {{difference_total | pct}}
                        </span>
                        </div>
                    </b-col>
                </div>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    import moment from 'moment-business-days'

    export default {
        name: "FreeCredit",
        props: {
            due_date: {
                type: String,
                default: '2019-08-14'
            },

            current_month_debt: {
                type: Number,
                default: 14434
            },

            total_debt: {
                type: Number,
                default: 30000
            },

            income: {
                type: Number,
                default: 150000
            }

        },
        data() {
            return {
                today: new Date().toISOString().split('T')[0],
                days_left: null,
                difference_month: null,
                difference_total: null,
            }
        },
        methods: {
        },
        computed: {},
        filters: {
            currency_rub(amount) {
                return amount && `${amount.toLocaleString(undefined, {maximumFractionDigits: 2})}₽`
            },
            pct(amount) {
                return amount && `${amount.toLocaleString(undefined, {maximumFractionDigits: 2})}%`
            },
        },
        mounted() {
            this.days_left = moment(this.payday).diff(moment(this.today), 'd');
            this.difference_month = this.current_month_debt / this.income * 100;
            this.difference_total = this.total_debt / this.income * 100
        }
    }
</script>

<style lang="less">
    .price-holder {
        padding: 5px 10px;
        background-color: #d1d1d1;
        margin: 10px 0;
        width: fit-content;
        border-radius: 0.2em;
        color: white;
        text-align: center;
        font-size: 1.2em;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .label {
        font-size: 0.5em;
        padding: 4px;
        border-radius: 0.2em;
        font-weight: 900;

        &.up {
            background-color: #438a43;
        }

        &.down {
            background-color: #cc3232;
        }
    }
</style>