<template>
  <div class="main">
    <h1>Магазин</h1>
    <button type="button" v-on:click="openCart">Открыть корзину</button>
    <div class="catalog">
      <div class="card" v-for="(product, index) in catalog" v-bind:key="index">
        <div class="product-name">
          {{ product.name }}
        </div>
        <div class="price">
          {{ product.price }}
        </div>
        <button type="button" v-on:click="addToCart(product.id)">Купить</button>
      </div>
    </div>
  </div>
  <div class="cart" v-if="cartIsOpen">
    <div class="cart-wrapper">
      <div class="cart-title">Корзина</div>
      <ul class="cart-list">
        <li
          class="cart-item"
          v-for="(product, index) in cart"
          v-bind:key="index"
        >
          {{ product.name }} {{ product.price }} X {{ product.count }} Итого:
          {{ product.price * product.count }}
          <button type="button" v-on:click="changeCount(product.id, +1)">
            +
          </button>
          <button type="button" v-on:click="changeCount(product.id, -1)">
            -
          </button>
        </li>
      </ul>
      <div class="cart-total">Общая сумма: {{ cartTotal }}</div>
      <button class="cart-close-btn" v-on:click="closeCart">X</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      catalog: [
        { id: "1", name: "Кофта", price: 5000 },
        { id: "2", name: "Пальто", price: 7500 },
        { id: "3", name: "Штаны", price: 4000 },
        { id: "4", name: "Майка", price: 3000 },
        { id: "5", name: "Шапка", price: 2000 },
        { id: "6", name: "Рубашка", price: 4500 },
      ],

      cart: [],

      cartIsOpen: false,
    };
  },
  computed: {
    cartTotal() {
      let total = 0;
      this.cart.forEach((item) => {
        total += item.count * item.price;
      });
      return total;
    },
  },
  methods: {
    addToCart(id) {
      let product = this.catalog.find((item) => item.id === id);
      let addedProduct = this.cart.find((item) => item.id === product.id);
      if (addedProduct != null) {
        addedProduct.count++;
      } else {
        product.count = 1;
        this.cart.push(product);
      }
      console.log(this.cart);
    },
    changeCount(id, amount) {
      let productIndex = this.cart.findIndex((item) => item.id === id);
      console.log(productIndex);
      if (productIndex >= 0) {
        this.cart[productIndex].count += amount;
        if (this.cart[productIndex].count == 0) {
          this.cart.splice(productIndex, 1);
        }
      }
    },
    openCart() {
      this.cartIsOpen = true;
    },
    closeCart() {
      this.cartIsOpen = false;
    },
  },
};
</script>

<style>
body {
  margin: 0;
}
.main {
  max-width: 1200px;
  margin: 0 auto;
}
.catalog {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 10px;
}
.card {
  box-sizing: border-box;
  width: 150px;
  height: 200px;
  border: 1px solid black;
  padding: 30px;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  font-size: 20px;
}
.cart {
  width: 100%;
  height: 100vh;
  background: rgba(0, 0, 0, 0.315);
  position: absolute;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
}
.cart-wrapper {
  background: #fff;
  padding: 50px;
  position: absolute;
}
.cart-close-btn {
  position: absolute;
  top: 10px;
  right: 10px;
}
</style>
