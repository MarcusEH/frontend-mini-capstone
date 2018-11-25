<template>
  <div class="products-edit">
    <div class="container">
      <form v-on:submit.prevent="submit()">
        <h1>Edit Product</h1>
        <ul>
          <li class="text-danger" v-for="error in errors">{{ error }}</li>
        </ul>
        <div class="form-group">
          <label>Name:</label> 
          <input type="text" class="form-control" v-model="product.name">
        </div>
        <div class="form-group">
          <label>Price:</label>
          <input type="number" step="1" class="form-control" v-model="product.price">
        </div>
        <div class="form-group">
          <label>Description:</label>
          <input type="text" class="form-control" v-model="product.description">
        </div>
        <div class="form-group">
          <label>Supplier ID:</label>
          <input type="number" step="1" class="form-control" v-model="product.supplier_id">
        </div>
        <input type="submit" class="btn btn-primary" value="Submit">
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      product: {},
      // name: this.name,
      // price: this.price,
      // description: this.description,
      // supplier_id: this.supplier_id,

      errors: [],
      // edittedProduct: {name: this.name, price: 0, description: "", supplier_id: 1}
    };
  },
  methods: {
    submit: function() {
      var params = {
        input_name: this.product.name,
        input_price: this.product.price,
        input_description: this.product.description,
        input_supplier_id: this.product.supplier_id
      };
      axios
        .patch("http://localhost:3000/api/products/" + this.$route.params.id, params)
        .then(response => {
          // this.product.push(response.data);
          this.$router.push('/');
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    },
    created: function() {
      axios.get('http://localhost:3000/api/products/' + this.$route.params.id).then(response => {
        console.log(response.data);
        // this.name = response.data.name;
        // this.price = response.data.price;
        // this.description = response.data.description;
        // this.supplier_id = response.data.supplier_id;
        this.product = response.data;
      });
    }
  }
};
</script>