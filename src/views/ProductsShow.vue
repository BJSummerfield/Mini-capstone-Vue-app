<template>
  <div class="show">
    <h1>{{ message }}</h1>
    <hr>
      <img v-bind:src="product.image_url">
      <p>{{ product.name }}</p>
      <p>{{ product.description }}</p>
      <p>{{ product.formatted['price'] }}</p>
      <p>Tax: {{ product.formatted['tax'] }}</p>
      <p>Total: {{ product.formatted['total'] }}</p>
      <router-link v-bind:to="'/products/' + product.id + '/edit'">Edit the product</router-link>
      <hr>
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
      message: "Product",
      product: {}
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      console.log(response.data);
      this.product = response.data;
    });
  },
  methods: {}
};
</script>    