<template>
    <div>

    </div>
</template>

<script>
    export default {
        name: "FreeCredit",
        props: {
            //if known
            due_date: {
                type: String,
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

        }
    }
</script>

<style scoped>

</style>