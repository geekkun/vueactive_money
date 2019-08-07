<template>
    <div>

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