<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
<!--    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <DragPriorities></DragPriorities>
    <Currency></Currency>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import DragPriorities from '@/components/DragPriorities.vue'
import Currency from '@/components/Currency.vue'
export default {
  name: 'home',
  components: {
    HelloWorld,
    DragPriorities,
    Currency
  }
}
</script>
