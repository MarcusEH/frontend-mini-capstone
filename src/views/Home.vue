<template>
  <div class="home">
    <h1>Products</h1>
    <h5>Total products: {{products.length}}</h5>
    <p>Search for product (name or description): <input v-model="nameFilter" list="names"></p>
    <datalist id="names">
      <span v-for="product in products">
        <option>{{product.name}}</option>
        <option>{{product.description}}</option>
      </span>
    </datalist>
    <p> </p>
      <div class="container">
        <div class="row">
          <div class="col-sm-4" v-for="product in filterBy(products, nameFilter, 'name')">
            <div class="card">
              <div class="card-body">
                <p class="card-title">Name: {{product.name}}</p>
                <p class="card-title">ID: {{product.id}} </p>
                <p class="card-title">price: ${{product.price}}</p>
                <p class="card-text">description: {{product.description}}</p>
                <img v-bind:src="product.images" width= 100px>
                <p><a v-bind:href="'/#/products/' + product.id" class="btn btn-primary">More</a></p>
                <a v-bind:href="'/#/products/' + product.id + '/edit'" class="btn btn-primary">Edit</a>
              </div>
            </div>
          </div>
        </div>
      </div>

    <!-- 
    <div class="container">
      <div class="row">
        <div class="col-sm-4" v-for="product in products">
          <div class="card-body">
            <h3>Title: {{product.name}}</h3>
            <h4>ID: {{product.id}} </h4>
            <h4>price: {{product.price}}</h4>
            <h4>description: {{product.description}}</h4>
            <img v-bind:src="product.images" width= 100px>
            <a v-bind:href="'/#/products/' + product.id" class="btn btn-primary">More</a>
            <a v-bind:href="'/#/products/' + product.id + '/edit'" class="btn btn-primary">Edit</a>
          </div>
        </div>
      </div>
    </div> -->
    <h3>New Product</h3>
    <p class="red" v-for="error in errors">{{error}}</p>
    <p>name: <input type="text" v-model="newProduct.name"></p>
    <p>price: <input type="number" v-model="newProduct.price"></p>
    <p>description: <input type="text" v-model="newProduct.description"></p>
    <p>supplier_id: <input type="number" v-model="newProduct.supplier_id"></p>
    <button v-on:click="addProduct()">Add new product</button>
  </div>
</template>

<style>
.red {
  color: red;
}
</style>

<script>
var axios = require('axios');
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      products: [],
      newProduct: {name: "", price: 0, description: "", supplier_id: 1},
      errors: [],
      nameFilter: ""
    };
  },
  created: function() {
    axios.get('http://localhost:3000/api/products').then(function(response) {
      console.log(response.data);
      this.products = response.data;
    }.bind(this));
  },
  methods: {
    addProduct: function() {
      var params = {
        input_name: this.newProduct.name,
        input_price: this.newProduct.price,
        input_description: this.newProduct.description,
        input_supplier_id: this.newProduct.supplier_id
      };
      axios.post('http://localhost:3000/api/products', params).then(function(response) {
        console.log(response.data);
        this.products.push(response.data);
        this.errors = [];
      }.bind(this)).catch(function(errors) {
        console.log('in the errors');
        console.log(errors.response);
        this.errors = errors.response.data.errors;
      }.bind(this));
    }
  },
  computed: {}
};
</script>
