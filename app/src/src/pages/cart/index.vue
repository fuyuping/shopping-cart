<template>
  <div class="page-cart">
    <div class="title">总价 {{totalPrice}}</div>
    <div class="list">
      <cart-item
        v-for="(product, index) in cartProducts"
        :key="index"
        :product="product"
        @amount-change="handleAmountChange(index, arguments)"
      />
    </div>
    <add-product @add="handleAddProduct" />
  </div>
</template>

<script>
  import cartItem from './components/cartItem'
  import addProduct from './components/addProduct'

  const generateProduct = code => ({
    code,
    amount: 1,
    price: 100,
  })
  export default {
    name: 'cart',
    components: {
      cartItem,
      addProduct,
    },
    data() {
      return {
        code: '',
        shouldShowCart: false,
        cartProducts: [],
      }
    },
    computed: {
      totalPrice() {
        return this.cartProducts.reduce((prev, item) => {
          return prev + item.price * item.amount
        }, 0)
      },
    },
    methods: {
      handleAmountChange(index, agrs) {
        const amount = agrs[0]
        this.cartProducts[index].amount = amount
      },
      handleAddProduct(code) {
        const index = this.cartProducts.findIndex(product => product.code === code)
        if (index >= 0) {
          this.cartProducts[index].amount += 1
        } else {
          this.cartProducts.push(generateProduct(code))
        }
      },
    },
  }
</script>

<style src="./cart.css"></style>
