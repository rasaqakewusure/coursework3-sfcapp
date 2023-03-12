<template>
    <div>
        <h2><u>Shopping Cart</u></h2>
        <h2>Your current items in the cart are
            ({{ totalItemsInTheCart }} in total): </h2>
            <div>
        <div v-for="subject in sortedProducts" class="shoppingcart">
            <div v-if="cartCount(subject._id) > 0">
                <p> Subject: {{ subject.subject }}</p>
                <p> Location: {{ subject.location }}</p>
                <p> Price: £ {{ subject.price }}</p>
                <img v-bind:src="subject.image">
                <p> Quantity: {{ cartCount(subject._id) }}</p>
                <button @click="removeCart(subject)" class="button2" v-if="cartCount(subject._id) > 0">Empty
                 Cart</button>
                </div>
            </div>
            </div>
            <!--COMMENT OUT HERE IF GOES WRONG-->
            <!--h1>Welcome to the Checkout</h1>   
            <p>
                <strong>Name: </strong>
                <input type="name" v-model.trim="order.name" id="name" required>
            </p>
            <p>
                <strong>Phone Number: </strong>
                <input type="phone" v-model.number="order.phone" id="phone" required>
            </p>
            <p>
                <strong>Surname: </strong>
                <input type="surname" v-model.trim="order.surname" id="surname" required>
            </p>
            <p>
                <strong>Address: </strong>
                <input type="address" v-model.trim="order.address" id="address" required>
            </p>
            <p>
                <strong>City: </strong>
                <input type="city" v-model.trim="order.city" id="city" required>
            </p>
            <p>
                <strong>State: </strong>
                <select v-model="order.state">
                    <option disabled value="">Select State</option>
                    <option v-for="(state, key) in states" v-bind:value="state">{{ key }}</option>
                </select>
            </p>
            <p>
                <strong>Zip/Postal Code:</strong><input type="text" v-model.number="order.zip" required />
            </p>
            <p>
                <input type="radio" id="home" value="Home" v-model="order.method">
                <label for="home">Home</label>
                <input type="radio" id="business" value="Business" v-model="order.method">
                <label for="business">Business</label>
            </p>

            <h2><u>Order Summary</u></h2>
            <p><strong>Name: </strong>{{ order.name }}</p>
            <p><strong>Phone Number: </strong>{{ order.phone }}</p>
            <p><strong>Surname: </strong>{{ order.surname }}</p>
            <p><strong>Address: </strong>{{ order.address }}</p>
            <p><strong>City: </strong>{{ order.city }}</p>
            <p><strong>State: </strong>{{ order.state }}</p>
            <p><strong>Zip/Postal Code: </strong>{{ order.zip }}</p>
            <p><strong>Method: </strong>{{ order.method }}</p>

            <button v-on:click="saveProductToDB">Place Order</button-->
        </div>
</template>
<script>
export default {
    name: "Checkout",
    props: ["sortedProducts", "imagesBaseURL", "cart"],
    data() {
        return {}
    },
    methods: {
        cartCount(id) {
            let count = 0;
            for (let i = 0; i < this.cart.length; i++) {
                if (this.cart[i] === id) {
                    count++;
                }
            }
            return count;
        },
        itemsLeft(subject) {
            return subject.availableInventory - this.cartCount(subject._id);
        },

        removeCart: function (subject) {
            this.$emit("remove-cart", subject);
},

        saveProductToDB() {
            var orders = [];

            this.sortedProducts.forEach(element => {
                var spaces = this.cartCount(element._id);
                if (spaces > 0) {

                    var orderData = { space: spaces, lessonId: element._id, name: this.order.name, surname: this.order.surname, address: this.order.address, city: this.order.city, state: this.order.state, zip: this.order.zip, method: this.order.method, phoneNumber: this.order.phone };
                    orders.push(orderData);

                    //set the url to your server and route
                    //One fetch that saves a new order with POST after it is submitted (1%).
                    //fetch("http://localhost:3000/collections/subjects", {
                    fetch("https://lessonsapp-env.eba-8pdfpj2u.us-east-1.elasticbeanstalk.com/collections/orders", {
                        method: "POST", //set the HTTP method as "POST"
                        headers: {
                            "Content-Type": "application/json", //set the data type as JSON
                        },
                        body: JSON.stringify(orderData) //need to stringify the JSON
                    }).then(
                        function (response) {
                            response.json().then(
                                function (json) {
                                    // webstore.orders.push(newOrder);
                                    console.log("Success: " + json.acknowledged);
                                }
                            )
                        }
                    );
                }
            });


            alert("Success: Order Inserted");
        },

    },

    computed: {
        totalItemsInTheCart: function () {
            return this.cart.length || "";
        },
        // sortedProducts() {
        //     function compare(a, b) {
        //         if (a.price > b.price) return 1;
        //         if (a.price < b.price) return -1;
        //         return -1
        //     }
        //     return this.subjects.sort(compare);
        // }
    }
}
</script>