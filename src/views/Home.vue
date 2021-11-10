<template>
  <div class="home">
    <h1>{{ message }}</h1>
    Name:
    <input type="text" v-model="newProductParams.name" />
    Description:
    <input type="text" v-model="newProductParams.description" />
    Image:
    <input type="text" v-model="newProductParams.image_url" />
    Price:
    <input type="integer" v-model="newProductParams.price" />
    <button v-on:click="createProduct()">Create Product</button>
    <div v-for="product in products" :key="product.id">
      <h3>{{ product.name }}</h3>
      <img v-bind:src="product.image_url" :alt="product.name" />
      <p>Price: ${{ product.price }}</p>
      <button v-on:click="showProduct(product)">More Info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product Info:</h1>
        <p>
          Name:
          <input type="text" v-model="currentProduct.name" />
        </p>
        <p>
          Description:
          <input type="text" v-model="currentProduct.description" />
        </p>
        <p>
          Price:
          <input type="text" v-model="currentProduct.price" />
        </p>
        <p>Tax: $ {{ currentProduct.tax }}</p>
        <p>Total: $ {{ currentProduct.total }}</p>
        <button v-on:click="updateProduct(currentProduct)">Update Product</button>
        <button>close</button>
      </form>
    </dialog>
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
      newProductParams: {},
      currentProduct: {},
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
      axios
        .post("http://localhost:3000/products", this.newProductParams)
        .then((response) => {
          console.log("Success!", response.data);
          this.products.push(response.data);
        })
        .catch((error) => console.log(error.response));
    },
    showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (product) {
      axios.patch("http://localhost:3000/products/" + product.id, product).then((response) => {
        console.log("Success", response.data);
      });
    },
  },
};
</script>
