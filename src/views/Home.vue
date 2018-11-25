<template>
  <div class="home">
    <h1>Products</h1>
    <h5>Total products: {{products.length}}</h5>
    <div class="container">
      <div class="row">
        <div class="col-sm-4" v-for="product in products">
          <div class="card h-300 card-body">
            <h2>Title: {{product.name}}</h2>
            <h4>ID: {{product.id}} </h4>
            <h4>price: {{product.price}}</h4>
            <h4>description: {{product.description}}</h4>
            <img v-bind:src="product.images" width= 100px>
            <a v-bind:href="'/#/products/' + product.id" class="btn btn-primary">More</a>
            <a v-bind:href="'/#/products/' + product.id + '/edit'" class="btn btn-primary">Edit</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
</style>

<script>
var axios = require('axios');
export default {
  data: function() {
    return {
      products: []
    };
  },
  created: function() {
    axios.get('http://localhost:3000/api/products').then(function(response) {
      console.log(response.data);
      this.products = response.data;
    }.bind(this));
  },
  methods: {},
  computed: {}
};
</script>
