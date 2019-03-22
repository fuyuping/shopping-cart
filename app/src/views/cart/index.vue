<template>
  <div class="page-cart">
    <div class="title">
      <span>商品数量 {{tollAmount}}</span>
      <span>选中商品的数量 {{Amount}}</span>
      <span>总价 {{totalPrice}}</span>
    </div>
    <div class="list">
      <cart-item
        v-for="(product, index) in cartProducts"
        :key="index"
        :product="product"
        @amount-change="handleAmountChange(product, arguments)"
        @select="handleResetProducts"
      />
    </div>
    <add-product @add="handleAddProduct" />
  </div>
</template>

<script>
  import { mapState, mapGetters, mapActions } from 'vuex'
  import cartItem from './components/cartItem'
  import addProduct from './components/addProduct'

  const generateProduct = (code, isSelected) => ({
    code,
    amount: 1,
    price: 100,
    isSelected
  })
  export default {
    name: 'cart',
    components: {
      cartItem,
      addProduct,
    },
    computed: {
      ...mapState({
        cartProducts: state => state.cart.cartProducts,
      }),
      ...mapGetters({
        totalPrice: 'cartTotalPrice',
        tollAmount: 'carttollAmount',
        Amount: 'cartAmount'
      }),
    },
    methods: {
      ...mapActions([
        'cartAddProduct',
        'cartChangeCount',
        'cartDelProduct',
        'cartChangeChecked'
      ]),
      handleAmountChange(product, agrs) {
        const amount = agrs[0]
        this.cartChangeCount({
          code: product.code,
          amount,
        })
      },
      handleAddProduct: function(code) {
        this.cartAddProduct(generateProduct(code, true))
      },
      handleResetProducts(code, isSelected) {
        console.log("状态变为：" + isSelected)
        this.cartChangeChecked(generateProduct(code, isSelected))
      }
    },
  }
</script>

<style src="./cart.css"></style>
