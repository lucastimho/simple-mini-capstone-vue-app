<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
</template>

<style></style>

<script>
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      data: [
        { id: 1, name: "Product 1", price: 10 },
        { id: 2, name: "Product 2", price: 230 },
      ],
    };
  },
  created: function () {},
  methods: {},
};
</script>
