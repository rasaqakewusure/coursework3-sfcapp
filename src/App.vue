<script setup>
import HelloWorld from
'./components/HelloWorld.vue'
import TheWelcome from
'./components/TheWelcome.vue'
</script>

<template>
  <div id="app">
  <header>
  <h1>{{sitename}}</h1>
  <button @click="showCheckout">{{totalItemsInTheCart}}
  <font-awesome-icon icon="fa-solid fa-cart-shopping" />
  Checkout
</button>
  </header>
  <main>
  <component :is="currentView"></component>
  </main>
  </div>
  </template>
  <script>
  import SubjectList from "./components/SubjectList.vue";
  import Checkout from "./components/Checkout.vue";
  
  export default {
name: "App",
data() { return { sitename: "Show all lessons",
cart: [],
currentView: SubjectList } },
components: { SubjectList, Checkout },
methods: { showCheckout() {
if (this.currentView === SubjectList) {this.currentView = Checkout}
else {this.currentView = SubjectList}
}}
,
computed: {
  totalItemsInTheCart: function () {
  return this.cart.length || "";
  },
  }

  };
  </script>

<style scoped>
  </style>