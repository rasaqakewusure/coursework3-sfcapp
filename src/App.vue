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
  <button @click="showCheckout">{{ this.cart.length }}
  Checkout</button>
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
  };
  </script>

<style scoped>
body{
  font-family: Arial, Arial, Arial;
  text-align: center;
}

h1{
  font-size: 250%;
}



#rcorners2 {
    border-radius: 25px;
    border: 2px solid navy;
    padding: 20px; 
    width: 230px;
    height: 230px;  
  }

  p{
    font-family: Arial, Arial, Arial;
    font-size: 120%;
  }

  img{
    width: 100px;
    height: 100px;
  }
  
  .button1{
    background-color: grey;
    font-size: 250%;
  }

  button{
    background-color: green;
    font-size: 140%;
  }

  html{
    background-color: rgb(218, 218, 196);
  }

  .button2{
    background-color: grey;
    font-size: 140%;

  }

  #star{
    width: 200px;
    height: 200px;
  }
  </style>