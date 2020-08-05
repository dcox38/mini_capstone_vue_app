<template>
  <div class="home">

    <h1>{{ message }}</h1>
    <h1>{{ name }}</h1>

    <button v-on:click="addProduct()">Create Product</button>
  
  
   
    <div v-for="product in products">
      <p>name: {{ product.name }}</p>
      <p>price: {{ product.price }}</p>
      <p>tax: {{ product.tax }}</p>
      <p>total: {{ product.total }}</p>
      <p>image_url: {{ product.image_url }}</p>
      <img v-bind:src="product.image_url">
      <br>
      <button v-on:click="showInfo(product)">Show me more</button>
      <hr> 
    </div>

    <dialog id="product-details">
      <form method="dialog">
        <h1>Product info</h1>
        <p>Name: {{currentProduct.Name}}</p>
        <p>Price: {{currentProduct.price}}</p>
        <p>Tax: {{currentProduct.tax}}</p>
        <p>Total: {{currentProduct.total}}</p>
        <p>Description: {{currentProduct.description}}</p>
        <button>Close</button>
      </form>
    </dialog> -->

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      name: "Danimal",
      products: [], 
      currentProduct: {}
    };
  },
  created: function() {
    this.indexProducts();
  },
  methods: {
    indexProducts: function() {
      console.log("why won't you print out!!!");
      console.log('products index...');
      
      axios.get('/api/products').then(response => {
        console.log(response);
        this.products = response.data;
      });
    },
    addProduct: function() {
      console.log('adding products...');
      var params = {
        name: "This is another new product!!!",
        price: 200,
        tax: "here are the directions",
        total: "here are the ingredients",
        image_url: "http://something.com",
        description: "It's a thing"
      };
      axios.post('/api/products', params).then(response => {
        console.log(response.data);
        this.products.push(response.data);
      });
    },
    showInfo: function(product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector('#product-details').showModal();
    }
  }
};
</script>

