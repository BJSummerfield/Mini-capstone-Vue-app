<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>price:<input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Image_url:<input type="text" v-model="newProductImage_url"></p>
    <button v-on:click="addNewProduct()">add a new product</button> 
    
    <p></p>
    <hr>
    <div v-for="product in products">
    <img v-bind:src="product.image_url">
    <p>{{ product.name }}</p>
    <p>{{ product.description }}</p>
    <p>{{ product.price }}</p>
    <hr>
    <!-- <p>{{ product.image.url }}</p> -->
  </div>
  </div>
</template>

<style>
  img {
    width: 90px;
  }
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImage_url: ""

    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    addNewProduct: function() {
      console.log('add new product...');
      
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newProductImage_url
      };
      console.log(params);
      axios.post("/api/products", params).then(response => {
        console.log(response.data);
        this.products.push(response.data);

      });
    }
  }
};
</script>
