<template>
  <div class="app-container">
    <Header/>
    <div class="main-container">
      <router-view
        :products="products"
        @viewProduct="handleViewProduct"
        :productInView="productInView"
        :status="status"
        @removeStatus="handleRemoveStatus"
        @addProductToCart="handleAddProductToCart"
        :cart="cart"
        @updateItemQuantity="handleUpdateItemQuantity"
        @removeItem="handleRemoveItem"
      />
    </div>
  </div>
</template>

<script>
import Header from "./components/Header";
export default {
  name: "App",
  props: {
    commerce: {
      type: Object,
      required: true
    }
  },
  components: {
    Header
  },
  data() {
    return {
      products: [],
      productInView: {},
      cart: {
        line_items: []
      },
      status: undefined
    };
  },
  methods: {
    handleViewProduct(product) {
      this.productInView = product;
      this.status = undefined;
    },
    handleUpdateItemQuantity(id, quantity) {
      this.commerce.cart
        .update(id, { quantity })
        .then(res => {
          this.cart = res.cart;
        })
        .catch(err => console.log(err));
    },
    handleRemoveItem(id) {
      this.commerce.cart
        .remove(id)
        .then(res => {
          this.cart = res.cart;
        })
        .catch(err => console.log(err));
    },
    handleAddProductToCart(product) {
      this.commerce.cart
        .add(product.id)
        .then(res => {
          this.status = "item successfully added to cart!";
          this.cart = res.cart;
        })
        .catch(err => console.log(err));
    },
    handleRemoveStatus() {
      this.status = undefined;
    }
  },
  created() {
    if (this.$route.params.productId && this.productInView) {
      this.commerce.products
        .retrieve(this.$route.params.productId)
        .then(res => {
          this.productInView = res;
        })
        .catch(err => console.log(err));
    }
    this.commerce.products
      .list()
      .then(res => {
        this.products = res.data;
      })
      .catch(err => console.log(err));

    this.commerce.cart
      .retrieve()
      .then(res => {
        this.cart = res;
      })
      .catch(err => console.log(err));
  }
};
</script>

<style scoped>
.main-container {
  width: 800px;
  margin: 0 auto;
}
@media (max-width: 700px) {
  .main-container {
    width: 100%;
  }
}
</style>
