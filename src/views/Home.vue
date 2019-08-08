<template>
    <div class="home">
        <img alt="Vue logo" src="../assets/logo.png">
        <Currency></Currency>
        <DailyLimit></DailyLimit>
        <FreeCredit></FreeCredit>
        <b-container fluid>
            <b-row class="justify-content-center">
                <div>
                    <Income></Income>
                </div>
                <div class="d-inline-flex p-2"></div>
                <div>
                    <DragPriorities></DragPriorities>
                </div>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    // @ is an alias to /src
    // import HelloWorld from '@/components/HelloWorld.vue'
    import DragPriorities from '@/components/DragPriorities.vue'
    import Currency from '@/components/Currency.vue'
    import DailyLimit from '@/components/DailyLimit.vue'
    import FreeCredit from '@/components/FreeCredit.vue'
    import Income from '@/components/Income.vue'

    export default {
        name: 'home',
        components: {
            // HelloWorld,
            DragPriorities,
            Currency,
            DailyLimit,
            FreeCredit,
            Income
        }
    }
</script>
