<template>
  <div class="">
    <div class="seller-wrapper">
      <div class="title">{{seller.name}}</div>
      <div class="score">
        <vStar :size="12" :score="seller.score" class="star"></vStar>
        月售{{seller.sellCount}}单
      </div>
      <div class="label-wrapper">
        <div class="delivery-wrapper">
          <h1 class="title">起送价</h1>
          <div class="content">
            <span class="high-text">{{seller.minPrice}}</span>元
          </div>
        </div>
        <div class="delivery-wrapper">
          <h1 class="title">商家配送</h1>
          <div class="content">
            <span class="high-text">{{seller.deliveryPrice}}</span>元
          </div>
        </div>
        <div class="delivery-wrapper">
          <h1 class="title">平均配送时间</h1>
          <div class="content">
            <span class="high-text">{{seller.deliveryTime}}</span>分钟
          </div>
        </div>
      </div>
    </div>
    <vSplit></vSplit>
    <div class="seller-wrapper">
      <h1 class="title">公告与活动</h1>
      <p class="content">{{seller.bulletin}}</p>
      <div v-for="(support, index) in seller.supports" :key="index" class="dec-wrapper">
        <!-- <img :src="imgSrc[support.type]" /> -->
        <span :class="'icon'+support.type" class="icon" alt=""></span>
        <span class="text">{{support.description}}</span>
      </div>
    </div>
    <vSplit></vSplit>
    <div class="seller-wrapper">
      <h1 class="title">商家实景</h1>
      <div ref="pics">
        <div class="pic-wrapper" ref="picWrapper">
          <img v-for="(pic, index) in seller.pics" :key="index" :src="pic" class="pic">
        </div>
      </div>
      
    </div>
    <vSplit></vSplit>
    <div class="seller-wrapper">
      <h1 class="title">商家信息</h1>
      <div v-for="(info, index) in seller.infos" :key="index" class="info">
        {{info}}
      </div>
    </div>
  </div>
</template>

<script>
import vSplit from '@/components/split/split'
import vStar from '@/components/star/star'
import BScroll from 'better-scroll'

export default {
  components:{
    vSplit,
    vStar
  },
  props:{
    seller:{
      type:Object
    }
  },
  data () {
    return {
    }
  },
  watch: {
    seller() {
      this.$nextTick(()=>{
        this._initBScroll()
      })
    }
  },
  methods: {
    _initBScroll() {
      let width = this.seller.pics.length * 126
      this.$refs.picWrapper.style.width = width+'px'
      // this.$nextTick 是一个异步函数，为了确保 DOM 已经渲染
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.pics, {
            startX: 0,
            click: true,
            scrollX: true,
            // 忽略竖直方向的滚动
            scrollY: false,
            eventPassthrough: "vertical"
          });
        } else {
          this.scroll.refresh();
        }
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style  lang="stylus" rel="stylesheet/stylus">
.seller-wrapper 
  padding:0 18px
  overflow: hidden
  .title 
    font-size:14px 
    color:rgb(7,17,27)
    line-height:14px 
    font-weight:700
    padding:18px 0 12px 0
  .score 
    font-size:10px 
    color:rgb(77,85,93)
    line-height:18px
    margin-bottom:18px
    .star 
      display: inline-block
  p 
    padding:0 12px 8px 12px
    font-size:12px 
    color:rgb(240,20,20)
    line-height:24px
    text-align :justify
  .label-wrapper 
    border-top:1px rgba(7,17,27,0.1) solid
    display:flex
    .delivery-wrapper 
      flex:1
      text-align:center 
      margin:18px 0
      border-right:1px rgba(7,17,27,0.1) solid
      &:last-child
        border:none
      .title 
        font-size:10px 
        color:rgb(147,153,159) 
        line-height:10px
        padding: 0 0 4px 0
      .content 
        font-size:10px 
        .high-text 
          font-size:24px
  .dec-wrapper 
    padding:16px
    border-top:1px rgba(7,17,27,0.1) solid
    .icon 
      display:inline-block
      width:16px
      height:16px
      vertical-align:bottom  
      &.icon0
        background:url(./decrease_4@2x.png)  no-repeat  
        background-size:16px 16px
      &.icon1,&.icon2
        background:url(./discount_4@2x.png)  no-repeat  
        background-size:16px 16px
      &.icon3,&.icon4
        background:url(./special_4@2x.png)  no-repeat  
        background-size:16px 16px
      &.icon5
        background:url(./invoice_4@2x.png)  no-repeat  
        background-size:16px 16px
      &.icon6
        background:url(./guarantee_4@2x.png)  no-repeat  
        background-size:16px 16px
    .text 
      display :inline-block
      font-size:12px 
      color:rgb(7,17,27)
      line-height:16px
  .info
    font-size:12px 
    color:rgb(7,17,27)
    line-height:16px
    padding:16px
    border-top:1px rgba(7,17,27,0.1) solid
  .pic-wrapper
    height:90px
    overflow:hidden
    margin-bottom:18px
    .pic 
      width:120px
      margin-right:6px
</style>