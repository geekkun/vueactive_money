<template>
    <div>
        <b-container class="bv-example-row">
            <b-row>
                <div v-for="(value, key) in rates" :key="key">
                    <b-col>
                        <div class="price-holder">
                            <span>{{key}} {{value | currency_rub}}</span>
                            <span
                                    class="label ml-2"
                                    :class="difference[key] >= 0 ? 'up' : 'down'">
                            {{difference[key] | pct}}</span>
                        </div>
                    </b-col>
                </div>
            </b-row>
        </b-container>
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
                yesterday: null,
                history: {},
                rates: {},
                difference: {}

            }
        },
        methods: {
            pollData() {
                this.polling = setInterval(() => {
                    axios
                        .get(this.currency_url + this.base_currency + '&symbols=' + this.active_currencies.toString())
                        .then(response => (this.cur = response['data']))
                        .then(resp => (this.rates = Object.assign({}, ...Object.keys(resp['rates']).map(k => ({[k]: 1 / resp['rates'][k]})))))

                    this.getYesterday()
                    this.getHistorical()
                    this.calculateDifference()

                }, 3000)
            },
            getHistorical() {
                axios
                    .get(this.history_url + this.yesterday + '?base=' + this.base_currency + '&symbols=' + this.active_currencies.toString())
                    .then(response => {
                        return response['data']['rates']
                    })
                    .then(response => (this.history = Object.assign({}, ...Object.keys(response).map(k => ({[k]: 1 / response[k]})))))
            },
            calculateDifference() {
                // this.difference = Object.assign({}, ...Object.keys(this.history).map(key => ({[key]: ((this.cur['data'][key] - this.history[key])/((this.cur['data'][key]+this.history[key])/2))*100})));
                for (var key of Object.keys(this.history)) {
                    let current = this.rates[key]
                    let historical = this.history[key]
                    let average = (current + historical) / 2
                    let diff = current - historical
                    this.difference[key] = diff / average * 100
                    // this.difference[key] = ((this.cur['data'][key] - this.history[key])/((this.cur['data'][key]+this.history[key])/2))*100
                }
            },
            getYesterday() {
                //Get last currencies date from api, get yesterday and convert to correct format
                this.yesterday = (d => new Date(d.setDate(d.getDate() - 1)))(new Date(this.cur['date'])).toISOString().split('T')[0];

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
        filters: {
            currency_rub(amount) {
                // const amt = Number(amount);
                return amount && `${amount.toLocaleString(undefined, {maximumFractionDigits: 2})}₽`
            },
            pct(amount) {
                // const amt = Number(amount);
                return amount && `${amount.toLocaleString(undefined, {maximumFractionDigits: 2})}%`
            },
        },
        mounted() {

            axios
                .get(this.currency_url + this.base_currency + '&symbols=' + this.active_currencies.toString())
                .then(response => (this.cur = response['data']))
                .then(response => {
                    this.rates = Object.assign({}, ...Object.keys(response['rates']).map(k => ({[k]: 1 / response['rates'][k]})))
                })
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