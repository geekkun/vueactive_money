<template>
    <div>
        <b-container fluid>
            <b-row class="justify-content-md-center">
                    <b-col>
                        <div class="price-holder">
                            <span>{{today}}</span>

                        </div>
                        <div class="price-holder">
                            <span>{{expected_payday}}</span>
                        </div>
                         <div class="price-holder">
                            <span>{{day}}</span>
                        </div>
                    </b-col>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    import moment from 'moment-business-days'
    export default {
        name: "DailyLimit",
        props: {
            expected_payday: {
                type: String,
            },
            days_to_pay: {
                type: Number,
                default: 7
            },
            business_days:{
                type: Boolean,
                default: false
            },
            always_late:{
                type: Boolean,
                default: true
            }
        },
        data() {
            return {
                // today: new Date().toISOString().split('T')[0],
                today: new Date().toISOString().split('T')[0],
                day: null
            }},
        methods: {
            expect_payday(){
                let firstday = moment().startOf('month').format('YYYY-MM-DD')
                let payday = null
                if (this.business_days) {
                     payday = moment(firstday, 'YYYY-MM-DD').businessAdd(this.days_to_pay).toISOString().split('T')[0]
                } else{
                    payday = moment(firstday, 'YYYY-MM-DD').add(this.days_to_pay, 'd').toISOString().split('T')[0]
                }
                if (this.always_late){
                    this.expected_payday = moment(payday, 'YYYY-MM-DD').businessAdd(1).toISOString().split('T')[0]
                } else{
                    this.expected_payday = payday
                }
            }
        },
        computed: {
            // expect_payday(){
            //     // eslint-disable-next-line vue/no-side-effects-in-computed-properties
            //     if(this.expected_payday === undefined) {
            //         this.expected_payday= this.today;
            //     }
            // }
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
            // this.day = moment(this.today, 'YYYY-MM-DD').businessAdd(3).toISOString().split('T')[0]
            // this.day =  moment(this.today, 'YYYY-MM-DD').format('YYYY-MM')
            let firstday = moment().startOf('month').format('YYYY-MM-DD')
            let payday = moment(firstday, 'YYYY-MM-DD').add(this.days_to_pay, 'd').toISOString().split('T')[0]
            this.day = moment(payday, 'YYYY-MM-DD').nextBusinessDay().toISOString().split('T')[0]
            // if(this.expected_payday === undefined) {
            // this.expected_payday= this.today;}
            this.expected_payday = payday
            // this.expect_payday()
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