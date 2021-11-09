<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <button v-on:click="createProduct()">Create Product</button>
    <div v-for="product in products" :key="product.id">
      <h3>{{ product.name }}</h3>
      <p>Price: ${{ product.price }}</p>
      <img v-bind:src="product.image_url" :alt="product.name" />
    </div>
  </div>
</template>

<style>
img {
  width: 250px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Lucas's Product Page!",
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products").then((response) => {
        this.products = response.data;
        console.log("All products", this.products);
      });
    },
    createProduct: function () {
      var params = {
        name: "test",
        description: "example",
        image_url: "image.png",
        price: 1,
      };
      axios
        .post("http://localhost:3000/products", params)
        .then((response) => {
          console.log("Success!", response.data);
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
  },
};
</script>
