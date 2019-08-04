<template>
    <div>
        <div v-for="(value, key) in rates" :key="key" v-if="active_currencies.includes(key)">{{key}}:{{value}}
            <!--        <div v-for="(value, key) in filteredCurrencies" :key="key">{{key}}:{{value}}-->
        </div>
        {{history}}
    </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: "Currency",
        props: {
            active_currencies: {
                default: () => ['USD', 'GBP', 'EUR'],
                type: Array,
            },
            base_currency: {
                default: 'RUB',
                type: String,
            }
        },
        data() {
            return {
                interval: 1000,
                currency_url: 'https://api.exchangeratesapi.io/latest?base=',
                history_url: 'https://api.exchangeratesapi.io/',
                cur: null,
                polling: null,
                history: null,
                rates: null

            }
        },
        methods: {
            pollData() {
                this.polling = setInterval(() => {
                    axios
                        .get(this.currency_url + this.base_currency)
                        .then(response => (this.cur = response['data']))
                        .then(response => (this.rates = Object.assign({}, ...Object.keys(response['rates']).map(k => ({[k]: 1/response['rates'][k]}))) ))
                    this.getHistorical()
                }, 3000)
            },
            getHistorical() {
                axios
                    .get(this.history_url + this.cur['date'] + '?base=' + this.base_currency + '&symbols=' + this.active_currencies.toString())
                    .then(response => {return response['data']['rates']})
                    .then(response => (this.history = Object.assign({}, ...Object.keys(response).map(k => ({[k]: 1/response[k]}))) ))
            },
            calculateDifference() {
                let result = null;
                // let newObj = Object.assign({}, ...Object.keys(obj).map(k => ({[k]: obj[k] * obj[k]})));
                for (var key of Object.keys(this.history)) {
                    result[key] = (this.cur['data'][key] - this.history[key]) / this.history[key]
                }

            }

        },
        computed: {
            // filteredCurrencies() {
            //     let curnc = this.cur['rates']
            //     // let filteredCurrencies = curnc.filter((story) => {
            //     //     return story.author.toLowerCase().includes(this.searchTerm.toLowerCase());
            //     // })
            //     let filteredCurrencies = Object.fromEntries(Object.entries(curnc).filter(([key, v]) => this.active_currencies.includes(key)));
            //
            //     return filteredCurrencies;
            // }
        },
        mounted() {

            axios
                .get(this.currency_url + this.base_currency)
                .then(response => (this.cur  = response['data']))
                .then(response => (this.rates = Object.assign({}, ...Object.keys(response['rates']).map(k => ({[k]: 1/response['rates'][k]}))) ))

            // this.cur = Object.assign({}, ...Object.keys(this.cur['rates']).map(k => ({[k]: 1/this.cur['rates'][k]})))
        },
        beforeDestroy() {
            clearInterval(this.polling)
        },
        created() {
            this.pollData()
            // this.getHistorical()
        }


    }
</script>

<style scoped>

</style>