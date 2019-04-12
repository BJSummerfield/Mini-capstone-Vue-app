git ProductsEdit.vue<template>
  <div class="edit">
    <h1>{{ message }}</h1>
    <hr>
   
      <form v-on:submit.prevent="updateProduct()">
        <p>name: <input type="text" v-model="product.name"></p>
        <p>description: <input type="text" v-model="product.description"></p>
        <p>price: <input type="text" v-model="product.price"></p>
        <p>image_url: <input type="text" v-model="product.image_url"></p>
        <input type="submit" value="Update Product">
      </form>
       <button v-on:click="deleteProduct()">Delete Product</button>
        <div v-for="error in errors">
      <p>{{ error }}</p>
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
      message: "Edit Product",
      product: {
        name: "",
        price: "",
        description: "",
        image_url: "",
      },
      errors: []
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      console.log(response.data);
      this.product = response.data;
    });
  },
  methods: {
 
    updateProduct: function(theProduct) {
      console.log('updating the product...');
      var params = {
        name: this.product.name,
        price: this.product.price,
        description: this.product.description,
        image_url: this.product.image_url
      };

      console.log(params);

      axios.patch("/api/products/" + this.$route.params.id, params).then(response => {
        console.log('things are going well');
        console.log(response);
        this.$router.push("/products/" + this.$route.params.id);
      }).catch(error => {
        console.log('things are going poorly');
        console.log(error.response.data.errors);
        this.errors = error.response.data.errors;
      });
    },
    

    deleteProduct: function() {
      console.log('deleting the product...');
      axios.delete("/api/products/" + this.$route.params.id).then(response => {
        this.$router.push("/");
      });
    }
  }
};
</script>    