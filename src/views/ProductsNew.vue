<template>
  <div class="ProductsNew">
    
    <h1>{{ message }}</h1>
    <hr>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>price:<input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Image_url:<input type="text" v-model="newProductImage_url"></p>
    <button v-on:click="addNewProduct()">Add a new product</button> 
    <p></p>
    <hr>
    <div v-for="error in errors">
      {{ error }}
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
      message: "New Product",
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImage_url: "",
      errors: []
    };
  },
  created: function() {},
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
        this.$router.push("/");
      }).catch(error => {
        console.log(error.response.data.errors);
        this.errors = error.response.data.errors;
      });
    },
  }
};
</script>    