<template>
    <div class="home">
        <img alt="Vue logo" src="../assets/logo.png">
        <Currency></Currency>
        <DailyLimit></DailyLimit>
        <FreeCredit></FreeCredit>
        <!--        <DragPriorities></DragPriorities>-->
    </div>
</template>

<script>
    // @ is an alias to /src
    // import HelloWorld from '@/components/HelloWorld.vue'
    // import DragPriorities from '@/components/DragPriorities.vue'
    import Currency from '@/components/Currency.vue'
    import DailyLimit from '@/components/DailyLimit.vue'
    import FreeCredit from '@/components/FreeCredit.vue'

    export default {
        name: 'home',
        components: {
            // HelloWorld,
            // DragPriorities,
            Currency,
            DailyLimit,
            FreeCredit
        }
    }
</script>
