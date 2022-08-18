<template>
  <div class="container">
    <add-product :addProduct="addProduct" :products="products" />
    <products-gallery :deleteProduct="deleteProduct" :products="products" />
  </div>
</template>

<script>
/* eslint-disable */
import AddProduct from "./components/AddProduct.vue";
import ProductsGallery from "./components/ProductsGallery.vue";

export default {
  name: "App",
  components: {
    AddProduct,
    ProductsGallery,
  },
  data() {
    return {
      products: [],
    };
  },
  methods: {
    addProduct: function (product) {
      this.products.push(product);
      this.saveProducts();
    },
    deleteProduct: function (product) {
      this.products.splice(this.products.indexOf(product), 1);
      this.saveProducts();
    },
    saveProducts: function () {
      const parsed = JSON.stringify(this.products);
      localStorage.setItem("products", parsed);
    },
  },
  mounted() {
    if (localStorage.getItem("products")) {
      try {
        this.products = JSON.parse(localStorage.getItem("products"));
      } catch (e) {
        localStorage.removeItem("products");
      }
    }
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Inter:wght@600&family=Source+Sans+Pro:wght@400;600&display=swap");
@import './assets/scss/reset';
@import './assets/scss/vars';
* {
  font-family: "Source Sans Pro", sans-serif;
  font-size: $regular;
  box-sizing: border-box;
  color: $black;
}

.container {
  padding: 32px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

::-webkit-scrollbar {
  width: 0;
}
</style>
