<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>price:<input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Image_url:<input type="text" v-model="newProductImage_url"></p>
    <button v-on:click="addNewProduct()">Add a new product</button> 
    <p></p>
    <hr>
    
    <div v-for="product in products">
      <img v-bind:src="product.image_url">
      <p><button v-on:click="toggleInfo(product)">More Info</button>
        </p>
      <p>{{ product.name }}</p>
      <p>{{ product.formatted['price'] }}</p>
      <div v-if="product === currentProduct">
        <p>{{ product.description }}</p>
        <p>Tax: {{ product.formatted['tax'] }}</p>
        <p>Total: {{ product.formatted['total'] }}</p>
        <p>name: <input type="text" v-model="product.name"></p>
        <p>description: <input type="text" v-model="product.description"></p>
        <p>price: <input type="text" v-model="product.price"></p>
        <p>image_url: <input type="text" v-model="product.image_url"></p>
         <button v-on:click="updateProduct(product)">Update Product</button>
         <button v-on:click="deleteProduct(product)">Delete Product</button>

      </div>
      <hr>
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
      newProductImage_url: "",
      currentProduct: []
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
    },
    toggleInfo: function(theProduct) {
      if (this.currentProduct === theProduct) {
        this.currentProduct = {};
      } else { 
        this.currentProduct = theProduct;
      }
    },
    updateProduct: function(theProduct) {
      console.log('updating the product...');
      var params = {
        name: theProduct.name,
        price: theProduct.price,
        description: theProduct.description,
        image_url: theProduct.image_url
      };
      axios.patch("/api/products/" + theProduct.id, params).then(response => { 
        console.log(response);
        theProduct = response.data;
      });
    },
    deleteProduct: function(theProduct) {
      console.log('deleting product');
      var params = {
        id: theProduct.id
      };
      var index = this.products.indexOf(theProduct);
      console.log(index);
      this.products.splice(index, 1);
      
      axios.delete("/api/products/" + theProduct.id, params).then(response => {
        console.log(response);
        theProduct = response.data;
      });
    }
  }
};
</script>    