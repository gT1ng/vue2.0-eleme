<template>
<div class="food-container" v-show="showFlag" ref="food">
  <div class="food">
    <div class="image">
      <img :src="food.image" alt="">
      <div class="back" @click="back">
        <i class="icon-arrow_lift"></i>
      </div>
    </div>
    <div class="title">
      <h1 class="name">{{food.name}}</h1>
      <span class="sell">月售{{food.sellCount}}份 好评率{{food.rating}}%</span>
      <div class="price">
        ￥{{food.price}}
        <span class="old-price">{{food.oldPrice}}</span>
      </div>
      <div class="addCart" @click="addNum" v-show="!food.count">加入购物车</div>
      <vCount class="count" :food="food" v-show="food.count"></vCount>
    </div>
    <vSplit></vSplit>
    <div class="info">
      <h1 class="title">商品介绍</h1>
      <p class="text">{{food.info}}</p>
    </div>
    <vSplit></vSplit>
    <div class="ratings">
      <h1 class="title">商品评价</h1>
      <vRatingsControl :ratings="food.ratings" @ifContent="ifContent" @selectType="selectType"></vRatingsControl>
      <ul class="list-wrapper" v-show="food.ratings && food.ratings.length">
        <li v-for="(rating, index) in food.ratings" :key="index" v-show="needShow(rating.rateType, rating.text)">
          <div class="content">
            <span class="rateTime">{{getLocalTime(rating.rateTime)}}</span>
            <img class="avatar" :src="rating.avatar" alt="">
            <span class="username">{{rating.username}}</span>
          </div>
          <div class="text">
            <span class="icon-thumb_up" v-show="rating.rateType==0"></span>
            <span class="icon-thumb_down" v-show="rating.rateType==1"></span>
            {{rating.text}}
          </div>
        </li>
      </ul>
    </div>
  </div>
</div>
  
</template>

<script>
import vSplit from '@/components/split/split'
import vCount from '@/components/count/count'
import BScroll from 'better-scroll'
import vRatingsControl from '@/components/ratingsControl/ratingsControl'

export default {
  components: {
    vSplit,
    vCount,
    vRatingsControl
  },
  props: {
    food: {
      type: Object
    }
  },
  data () {
    return {
      showFlag: false,
      type: 2
    }
  },
  methods: {
    show() {
      this.showFlag = true;
      this.onlyContent = false;
      this.$nextTick(() => {
          if (!this.scroll) {
            this.scroll = new BScroll(this.$refs.food, {
              click: true
            });
          } else {
            this.scroll.refresh();
          }
        });
    },
    back() {
      this.showFlag = false
    },
    addNum(event) {
        if(!this.food.count) {
            this.$set(this.food,'count',1)
        }
    },
    ifContent(hasContent) {
      this.onlyContent = hasContent
    },
    selectType(type) {
      this.type = type
    },
    needShow(type, text) {
      if(this.onlyContent && !text) return false;
      if(this.type == 2 ) return true;
      else return this.type ==type;
    },
    getLocalTime(nS) { 
      //将时间戳（十三位时间搓，也就是带毫秒的时间搓）转换成时间格式
      // d.cTime = 1539083829787
      let date = new Date(nS),
      year = date.getFullYear(),
      month = date.getMonth()+1,
      day = date.getDate(),
      hour = date.getHours(),
      minute = date.getMinutes(),
      second = date.getSeconds()

      month = month < 10 ? "0"+month:month;
      day = day < 10 ? "0"+day:day;
      date = year+'-'+month+'-'+day+' '+ hour + ':' + minute + ':' + second;
      return date;
    }
  }
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
.food-container 
  position: fixed 
  top: 0 
  left: 0
  bottom: 48px
  background: #fff
  .image 
    position: relative
    img  
      width: 100%
    .back
      position: absolute 
      top: 10px 
      left: 0
      i
        display: block
        padding: 10px
        font-size: 20px
        color: #fff
  .title
    padding: 18px
    position: relative
    .name 
      font-size: 14px 
      font-weight: 700 
      color: rgb(7, 17, 27)
      line-height: 14px
    .sell 
      font-size: 10px 
      color: rgb(147, 153, 159)
      line-height: 14px
      padding: 8px 0 18px 0
      display: block
    .price 
      font-size: 14px 
      font-weight: 700 
      color: rgb(240, 20, 20)
      line-height: 24px
      &.old-price 
        font-size: 10px 
        font-weight: normal 
        color: rgb(147, 153, 159)
    .addCart 
      position: absolute
      display: inline-block
      right: 18px 
      bottom: 24px
      font-size: 10px 
      color: #fff 
      line-height: 12px
      padding: 6px 12px 
      border-radius: 12px
      background: rgb(0, 160, 220)
    .count 
      position: absolute
      right: 18px 
      bottom: 24px
  .info 
    padding: 18px
    .title 
      font-size: 14px 
      color: rgb(7, 17, 27)
      line-height: 24px
      padding: 0
    .text 
      font-size: 12px 
      color: rgb(77, 85, 93)
      line-height: 24px
  .ratings
    padding: 18px
    .title 
      font-size: 14px 
      color: rgb(7, 17, 27)
      line-height: 24px
      padding: 0
    .list-wrapper
      border-top: 1px rgba(7, 17, 27, 0.1) solid
      li 
        padding: 16px 0
        border-bottom: 1px rgba(7, 17, 27, 0.1) solid
        .content
          margin-bottom: 6px
          .rateTime
            font-size: 10px 
            color: rgb(147, 153, 159)
            line-height: 12px
          .username
            font-size:10px 
            color: rgb(147, 153, 159)
            line-height: 12px
            float: right
          .avatar
            width: 12px
            height: 12px
            float: right
            margin-left: 6px
        .text
          font-size: 12px 
          color: rgb(7, 17, 27)
          line-height: 16px
          .icon-thumb_up
            color: #00a0dc
</style>
