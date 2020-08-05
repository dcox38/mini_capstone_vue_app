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
        <p>Name: <input v-model="currentProduct.name"></p>
        <p>Price: <input v-model="currentProduct.price"></p>
        <p>Tax: <input v-model="currentProduct.tax"></p>
        <p>Total: <input v-model="currentProduct.total"></p>
        <p>Description: <input v-model="currentProduct.description"></p>
        <p>Image: <input v-model="currentProduct.image_url"></p>
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button>Close</button>
        <button v-on:click="destroyProduct(currentProduct)">Delete Product</button>
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
    },
    updateProduct: function(product) {
      console.log(product);

      var params = {
        name: this.currentProduct.name,
        price: this.currentProduct.price,
        tax: this.currentProduct.tax,
        total: this.currentProduct.total,
        description: this.currentProduct.description,
        image_url: product.image_url
      };

      axios.patch('api/products/' + this.currentProduct.id, params).then(response => {
        console.log(response.data);
        this.currentProduct = response.data;
      });
    },
    destroyProduct: function(product) {
      console.log(product);

      axios.delete('api/products/' + product.id).then(response => {
        console.log(response.data);

        var index = this.products.indexOf(product);

        this.products.splice(index, 1);

        console.log(index);
      });


    }
  }
};
</script>

