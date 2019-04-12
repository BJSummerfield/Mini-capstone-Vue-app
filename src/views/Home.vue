<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <hr>
    <input type="text" v-model="nameFilter" list="names"><!-- This has no use at this moment -->
    <datalist id="names"> <!-- This has no use at this moment -->
    <option v-for="product in products">{{product.name}}</option>
    </datalist>
    <button v-on:click="changeSortTerm('name')">Sort by name</button>
    <button v-on:click="changeSortTerm('price')">Sort by price</button>
    <div class="card-group">
      <transition-group class="row" appear enter-active-class="animated rollIn" leave-active-class="animated zoomOutUp">
      <div v-for="product in orderBy(filterBy(products, nameFilter), sortTerm)" v-bind:key="product.id">
        <div class="card" style="width: 18rem;">
          <div class="card-body">
            <h5 class="card-title">{{ product.name }}</h5>
            <p class="card-text">{{ product.formatted['price'] }}</p>
            <button v-on:click="removeProduct(product)">Remove</button>
            <a v-bind:href="'/products/' + product.id" class="btn btn-primary">More Info</a> 
          </div>
        </div>      
      </div>
    </transition-group>
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
import Vue2Filters from 'vue2-filters'
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Products",
      products: [],
      nameFilter: "",
      sortTerm: ""
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    });
  },
  methods: {
    removeProduct: function(product) {
      var index = this.products.indexOf(product);
      this.products.splice(index,1);
    },
    changeSortTerm: function(input) {
      console.log(input);
      this.sortTerm = input;  
    }
  }
};
</script>    