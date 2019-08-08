<template>
    <div>
        <b-container fluid>
            <b-row class="justify-content-md-center">
                <h3>Daily Money</h3>
            </b-row>
            <b-row class="justify-content-md-center">
                <div>
                    <b-col>
                        <div class="price-holder">
                        <span>
                            {{days_left}} day<span v-if="days_left>1">s</span>:  {{money_left | currency_rub}}
                        </span>
                        </div>
                    </b-col>
                </div>
                <div>
                    <b-col>
                        <div class="price-holder">
                            <span>You can spend {{daily_limit | currency_rub}} today!</span>
                            <span
                                    class="label ml-2"
                                    :class="difference >= 0 ? 'up' : 'down'">
                            {{difference | pct}}
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
        name: "DailyLimit",
        props: {
            //if known
            expected_payday: {
                type: String,
                default: '2019-09-08'
            },
            //set in contract
            days_to_pay: {
                type: Number,
                default: 7
            },
            //calculations based only on business days
            business_days: {
                type: Boolean,
                default: false
            },

            //maybe change to number?
            always_late: {
                type: Boolean,
                default: true
            },

            money_left: {
                type: Number,
                default: 28999
            },

            initial_cash: {
                type: Number,
                default: 30000
            }

        },
        data() {
            return {
                // today: new Date().toISOString().split('T')[0],
                today: new Date().toISOString().split('T')[0],
                payday: null,
                days_left: null,
                difference: null,
                daily_limit: null,
                previous_payday: null,
                full_period_length: null,
                initial_daily_limit: null,
            }
        },
        methods: {
            calculate_payday() {
                if (this.expected_payday === undefined) {
                    let firstday = moment().startOf('month').format('YYYY-MM-DD');
                    let payday = null;
                    if (this.business_days) {
                        payday = moment(firstday, 'YYYY-MM-DD').businessAdd(this.days_to_pay).toISOString().split('T')[0]
                    } else {
                        payday = moment(firstday, 'YYYY-MM-DD').add(this.days_to_pay, 'd').toISOString().split('T')[0]
                    }
                    if (this.always_late) {
                        this.payday = moment(payday, 'YYYY-MM-DD').businessAdd(2).toISOString().split('T')[0]
                    } else {
                        this.payday = payday
                    }
                } else {
                    this.payday = this.expected_payday
                }
                this.days_left = moment(this.payday).diff(moment(this.today), 'd');
                this.previous_payday = moment(this.payday).subtract(1, 'months');
                this.full_period_length = moment(this.payday).diff(this.previous_payday, 'd')

            },
            daily_money_limit() {
                this.daily_limit = this.money_left / this.days_left;
                this.initial_daily_limit = this.initial_cash / this.full_period_length;

                let average = (this.daily_limit + this.initial_daily_limit) / 2;
                let diff = this.daily_limit - this.initial_daily_limit;
                this.difference = diff / average * 100

            }
        },
        computed: {

        },
        filters: {
            currency_rub(amount) {
                return amount && `${amount.toLocaleString(undefined, {maximumFractionDigits: 2})}₽`
            },
            pct(amount) {
                return amount && `${amount.toLocaleString(undefined, {maximumFractionDigits: 2})}%`
            },
        },
        mounted() {
            this.calculate_payday();
            this.daily_money_limit()
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