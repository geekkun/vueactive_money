<template>
    <p>{{cur}}</p>
</template>

<script>
    import axios from 'axios'

    export default {
        name: "Currency",
        data() {
            return {
                interval: 1000,
                currency_url: 'https://api.exchangeratesapi.io/latest?base=',
                base_currency: 'RUB',
                cur: null
            }
        },
        // mounted() {
        //     this.interval = setInterval(() => {
        //         axios.get(this.my_url+this.base_currency).then(res => { /* do something */
        //             this.cur = res.json()
        //         });
        //     }, 30000);
        // },
        mounted() {
            axios
                .get(this.currency_url+this.base_currency)
                .then(response => (this.cur = response['data']['rates']))
        }


    }
</script>

<style scoped>

</style>