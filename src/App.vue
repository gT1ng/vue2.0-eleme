<template>
  <div id="app">
    <vHeader />
    <div class="tab-wrapper">
        <router-link to="/goods" class="tab-item">商品</router-link>
        <router-link to="/ratings" class="tab-item">评价</router-link>
        <router-link to="/seller" class="tab-item">商家</router-link>
    </div>
    <router-view :seller="seller" keep-alive></router-view>
    
  </div>
</template>

<script>
import vHeader from '@/components/header/header'
import axios from 'axios'

export default {
  name: 'App',
  components:{
    vHeader
  },
  data() {
    return {
      jsonData: {},
      seller:{}
    }
  },
  created() {
    this.getSeller()
    
  },
  methods: {
    getSeller () {
        axios.get('/api/seller').then((res)=>{
            this.seller = res.data.data
        })
    }
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
  .tab-wrapper
    display:flex
    border-bottom:1px rgba(7,17,27,0.1) solid
    .tab-item
      flex:1
      font-size:14px
      color:rgb(7,17,27)
      line-height:40px
      text-align: center
      &.router-link-active 
        color:rgb(240, 20, 20)

</style>
