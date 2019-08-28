<template>
  <div class="transition_test">
    <div class="shop_cart">
      加入购物车动画(css实现):
      <div class="add">
        <button @click="addShopCart">add</button>
        <transition name="shop_cart">
          <img class="add_img" v-if="addShow" src="../star/star48_on@3x.png" alt="">
        </transition>
      </div>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'myTransition',
    components: {

    },
    data() {
      return {
        addShow: false
      }
    },
    created() {

    },

    methods: {
      addShopCart () {
        this.addShow = true
        setTimeout(() => {
          this.addShow = false
        }, 1000)
      }
    }

  }
</script>
<style scoped>
  .transition_test {
    text-align: left;
    padding-bottom: 300px;
  }
  .shop_cart {
    padding: 20px;
  }
  .shop_cart .add {
    position: relative;
    margin-top: 100px;
    width: 500px;
    display: inline-block;
  }
  .shop_cart .add button {
    position: absolute;
    z-index: 99;
  }
  .shop_cart .cart img {
    width: 100%;
  }
  .shop_cart .add_img {
    width: 100px;
    position: absolute;
    left: 600px;
    top: -160px;
  }


  /* CSS类名实现 */
  .shop_cart-enter-active {
    transition: all 1s ease-out;
  }
  .shop_cart-enter {
    opacity: 0;
    transform: scale(.5);
    transform: translate(-610px, 100px);
  }

</style>