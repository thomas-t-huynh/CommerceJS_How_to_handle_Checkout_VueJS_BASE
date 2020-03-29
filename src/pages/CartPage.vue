<template>
  <div>
    <h2>{{ ifEmpty }}</h2>
    <CartItem
      v-for="product in cart"
      :product="product"
      :key="product.id"
      @updateItemQuantity="updateItemQuantity"
      @removeItem="removeItem"
    />
    <hr>  
    <div class="cartPage-subTotal-div">
      <button class="btn btn-primary">🔒 Secure Checkout</button>
      <h3 class="cartPage-subTotal-amount">Cart Subtotal: $ {{ cartTotal }}</h3>
    </div>
  </div>
</template>

<script>
import CartItem from "../components/CartItem.vue";

export default {
  name: "CartPage",
  components: {
    CartItem
  },
  props: {
    cart: {
      type: Array
    }
  },
  methods: {
    updateItemQuantity(id, quantity) {
      this.$emit("updateItemQuantity", id, quantity);
    },
    removeItem(id) {
      this.$emit("removeItem", id);
    }
  },
  computed: {
    ifEmpty() {
      return this.cart.length === 0 ? "Cart is empty" : undefined;
    },
    cartTotal() {
      return this.cart.reduce(
        (acc, currentEl) => acc + currentEl.quantity * currentEl.price.raw,
        0
      );
    }
  }
};
</script>

<style scoped>

.cartPage-subTotal-amount {
  margin: 10px 0;
}

.cartPage-subTotal-div {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

@media (max-width: 700px) {
  .cartPage-subTotal-div {
    flex-direction: column;
    margin-left: 20px;
  }
}

button {
  width: 175px;
}

</style>

