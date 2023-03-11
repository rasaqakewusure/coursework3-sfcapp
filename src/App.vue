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
<div v-if="testConsole && showTestConsole">
  <button @click="deleteAllCaches" class="test-elem">
    <!--Look for icon for delete-->
                Delete All Caches
                </button>

                <button @click="reloadPage" class="test-elem">
                 <!--Look for icon for reload-->
                Reload Page
                </button>

                <button @click="unregisterAllServiceWorkers" class="test-elem">
                  <font-awesome-icon icon="fa-brands fa-uniregistry" />
                Unregister All ServiceWorkers
                </button>


            <strong class="test-elem">HTTPS Test: </strong>
            <a v-bind:href="serverURL" target="_blank">link</a>
</div>

  </header>
  <main>
  <component :is="currentView" :sortedProducts="sortedProducts" :imagesBaseURL="imagesBaseURL" :cart="cart" 
  @add-item-to-cart="addItemToCart"></component>
  </main>
  </div>
  </template>
  <script>
  import SubjectList from "./components/SubjectList.vue";
  import Checkout from "./components/Checkout.vue";

  //import subjects from "./assets/json/subjects.json";
  
  export default {
name: "App",
data() { return { 
sitename: "Show all lessons",
//subjects : subjects,
subjects: [],
imagesBaseURL:"",
//imagesBaseURL:"https://lessonsapp-env.eba-8pdfpj2u.us-east-1.elasticbeanstalk.com/collections/subjects",
serverURL:"https://lessonsapp-env.eba-8pdfpj2u.us-east-1.elasticbeanstalk.com/collections/subjects",
cart: [],
testConsole: true,
showTestConsole: true,
currentView: SubjectList } },
components: { SubjectList, Checkout },
created: function () {
               let webstore = this;
                //GET all lessons
              
                //fetch("http://localhost:3000/collections/products").then(
                fetch("https://lessonsapp-env.eba-8pdfpj2u.us-east-1.elasticbeanstalk.com/collections/subjects").then(
                    function (response) {
                        response.json().then(
                            function (json) {
                                //alert(json);
                                //console.log(json);
                                webstore.subjects = json;
                            }
                        )
                    }
                );
                  
                //if ("serviceWorker" in navigator) {
                //navigator.serviceWorker.register("serviceworker.js");
//}
            },
methods: { showCheckout() {
if (this.currentView === SubjectList) {this.currentView = Checkout}
else {this.currentView = SubjectList}
},
   deleteAllCaches() {
                    caches.keys().then(function(names) {
                        for (let name of names)
                        caches.delete(name);
                    });
                    console.log("All Caches Deleted");
                },

                //Reload page
            reloadPage() {
                window.location.reload();
            },

            addItemToCart: function (subject) {
                    this.cart.push(subject._id);
                },

            unregisterAllServiceWorkers() {
                navigator.serviceWorker.getRegistrations().then(function (registrations) {
                    for (let registration of registrations) {
                        registration.unregister()
                    }
                });
                console.log("ServiceWorkers Unregistered");
            },


},
computed: {
  totalItemsInTheCart: function () {
  return this.cart.length || "";
  },
  sortedProducts() {
  function compare(a, b) {
  if (a.price > b.price) return 1;
  if (a.price < b.price) return -1;
  return -1
  }
  return this.subjects.sort(compare);
}


}

}
  </script>

<style scoped>
  </style>