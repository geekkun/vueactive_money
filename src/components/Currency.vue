<template>
    <div>
        <div v-for="(value, key) in rates" :key="key">{{key}}:{{value}}
            <!--        <div v-for="(value, key) in filteredCurrencies" :key="key">{{key}}:{{value}}-->
        </div>
        {{history}}
        {{difference}}
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
                yesterday:null,
                history: {},
                rates: {},
                difference: {}

            }
        },
        methods: {
            pollData() {
                this.polling = setInterval(() => {
                    axios
                        .get(this.currency_url + this.base_currency+ '&symbols=' + this.active_currencies.toString())
                        .then(response => (this.cur = response['data']))
                        .then(resp => (this.rates = Object.assign({}, ...Object.keys(resp['rates']).map(k => ({[k]: 1/resp['rates'][k]}))) ))

                    this.getYesterday()
                    this.getHistorical()
                    this.calculateDifference()

                }, 3000)
            },
            getHistorical() {
                axios
                    .get(this.history_url + this.yesterday + '?base=' + this.base_currency + '&symbols=' + this.active_currencies.toString())
                    .then(response => {return response['data']['rates']})
                    .then(response => (this.history = Object.assign({}, ...Object.keys(response).map(k => ({[k]: 1/response[k]}))) ))
            },
            calculateDifference() {
                // this.difference = Object.assign({}, ...Object.keys(this.history).map(key => ({[key]: ((this.cur['data'][key] - this.history[key])/((this.cur['data'][key]+this.history[key])/2))*100})));
                for (var key of Object.keys(this.history)) {
                    let current = this.rates[key]
                    let historical = this.history[key]
                    let average = (current+historical)/2
                    let diff = current - historical
                    this.difference[key] =  diff/average*100
                    // this.difference[key] = ((this.cur['data'][key] - this.history[key])/((this.cur['data'][key]+this.history[key])/2))*100
                }
            },
            getYesterday(){
                //Get last currencies date from api, get yesterday and convert to correct format
                this.yesterday = ( d => new Date(d.setDate(d.getDate()-1)))(new Date(this.cur['date'])).toISOString().split('T')[0];

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
                .get(this.currency_url + this.base_currency+ '&symbols=' + this.active_currencies.toString())
                .then(response => (this.cur  = response['data']))
                .then(response => {this.rates = Object.assign({}, ...Object.keys(response['rates']).map(k => ({[k]: 1/response['rates'][k]})))})
                // .then(response => (this.rates = Object.fromEntries(Object.entries(response).filter(([key, v]) => this.active_currencies.includes(key)))))

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