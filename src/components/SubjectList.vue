<template>
    <div>
        <h1>Welcome to the Product List Page</h1>

        <div v-for="subject in sortedProducts">
                    <h2 v-text="subject.subject"></h2>

                    <figure>
                        <img v-bind:src="imagesBaseURL + subject.image"
                        class="smallimage">
                    </figure>
                    
                    <p v-text="subject.location"></p>
                    <p>Price: £{{subject.price}}</p>
                    
                    <span v-if="subject.availableInventory === cartCount(subject._id)">All out!</span>
                    <span v-else-if="subject.availableInventory - cartCount(subject._id) < 5">
                        Only {{subject.availableInventory - cartCount(subject._id)}} left!
                    </span>
                    <span v-else>Buy now!</span>
                    <button v-on:click="addItemToCart(subject)" v-if="canAddToCart(subject)">Add to cart</button>
                    <button disabled="disabled" v-else>
                        Add to cart
                    </button>
                    <div>
                        <span v-for="n in subject.rating" id="star">★</span>
                        <span v-for="n in 5 - subject.rating" id="star">☆</span>
                    </div>
                    </div>
    </div>
    </template>

<script>
export default {
name: "ProductList",
props: ["sortedProducts", "imagesBaseURL", "cart"],
data(){
    return {}
},
methods: {

    addItemToCart: function (subject) {
    this.$emit("add-item-to-cart" , subject)
    },

    itemsLeft(subject) {
    return subject.availableInventory - this.cartCount(subject._id);
    },
    
    canAddToCart(subject) {
    return subject.availableInventory > this.cartCount(subject._id);
    },
    
    cartCount(id) {
    let count = 0;
    for (let i = 0; i < this.cart.length; i++) {
    if (this.cart[i] === id) {
    count++;
}
}
return count;
}
}
}

</script>