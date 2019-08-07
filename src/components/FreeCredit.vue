<template>
    <div>
        <b-container fluid>
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
                                    :class="difference >= 0 ? 'up' : 'down'"
                                    class="label ml-2">
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
        name: "FreeCredit",
        props: {
            due_date: {
                type: String,
                default: '2019-08-14'
            },

            current_month_debt: {
                type: Number,
                default: 12345
            },

            total_debt: {
                type: Number,
                default: 30000
            }

        },
        data() {
            return {
                today: new Date().toISOString().split('T')[0],
                days_left: null
            }
        },
        methods: {

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
            this.days_left = moment(this.payday).diff(moment(this.today), 'd');
        }
    }
</script>

<style scoped>

</style>