<template>
  <div class="">
    <div class="rating-wrapper">
      <div class="left">
        <div class="num">{{seller.score}}</div>
        <div class="text">综合评分</div>
        <div class="percent">高于周边商家{{seller.rankRate}}%</div>
      </div>
      <dir class="right">
        <div class="rating">
          <span class="text">服务态度</span>
          <vStar class="star" :score="seller.serviceScore" :size="12"></vStar>
          <span class="score">{{seller.serviceScore}}</span>
        </div>
        <div class="rating">
          <span class="text">服务态度</span>
          <vStar class="star" :score="seller.foodScore" :size="12"></vStar>
          <span class="score">{{seller.foodScore}}</span>
        </div>
        <div class="rating">
          <span class="text">送达时间</span>
          {{seller.deliveryTime}}分钟
        </div>
      </dir>
    </div>
    <vSplit></vSplit>
    <div class="selectRating">
      <vRatingsControl :ratings="ratings" @ifContent="ifContent" @selectType="selectType"></vRatingsControl>
    </div>
    <ul class="ratings-wrapper">
      <li class="rating" v-for="(rating, index) in ratings" :key="index" v-show="needShow(rating.rateType, rating.text)">
        <img :src="rating.avatar" class="avatar">
        <div class="content">
          <div class="title">
            <span class="name">{{rating.username}}</span>
            <span class="rateTime">{{getLocalTime(rating.rateTime)}}</span>
          </div>
          <div class="time">
            <vStar class="star" :score="rating.score" :size="10"></vStar>
            <span class="deliverytime" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送到</span>
          </div>
          <p class="text">{{rating.text}}</p>
          <ul class="recommend-wrapper" v-show="rating.recommend && rating.recommend.length">
            <span class="icon-thumb_up"></span>
            <li class="recommend" v-for="(item,index) in rating.recommend" :key="index">
              {{item}}
            </li>
          </ul>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios'
import vStar from '@/components/star/star'
import vSplit from '@/components/split/split'
import vRatingsControl from '@/components/ratingsControl/ratingsControl'

export default {
  components:{
    vStar,
    vSplit,
    vRatingsControl
  },
  props:{
    seller: {
      type:Object
    }
  },
  data () {
    return {
      ratings: [],
      type: 2,
      onlyContent: false
    }
  },
  created() {
    this.getRatings()
  },
  methods: {
    getRatings() {
      axios.get('/api/ratings').then((res)=>{
          this.ratings = res.data.data
      })
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
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang='stylus' rel='stylesheet/stylus'>

  .rating-wrapper
    display:flex 
    margin:18px 0
    .left
      flex:0 0 140px
      text-align :center
      .num 
        font-size:24px 
        color:rgb(255,153,0)
        line-height:28px 
        font-weight:700
      .text 
        font-size:12px 
        color:rgb(7,17,27)
        line-height:12px
        margin-top:6px 
      .percent
        font-size:10px 
        color:rgba(7,17,27,0.5)
        line-height:10px 
        margin-top:8px 
    .right
      flex:1
      padding:0 24px 
      border-left:1px rgba(147,153,159,0.2) solid
      .rating 
        font-size:12px 
        color:rgb(147,153,159)
        line-height:18px
        margin-bottom:8px
        .text
          display:inline-block
          font-size:12px 
          color:rgb(7,17,27)
          line-height:18px
        .star 
          display:inline-block
        .score 
          font-size: 12px 
          color: rgb(225,153,0)
          line-height: 18px
  .selectRating
    padding:0 18px
  .ratings-wrapper
    padding:0 18px 18px 18px 
    border-top:1px rgba(7,17,27,0.1) solid
    .rating 
      display:flex 
      padding:18px 0
      border-bottom:1px rgba(7,17,27,0.1) solid
      .avatar
        width:28px 
        height:28px 
        flex:0 0 28px 
        border-radius:50%
      .content 
        flex:1
        margin-left:12px
        .title
          .name
            font-size:10px 
            color:rgb(7,17,27) 
            line-height:12px
          .rateTime 
            font-size:10px 
            color:rgb(147,153,159)
            line-height:12px
            float:right
        .time 
          margin:4px 0 6px 0
          .star
            display:inline-block 
          .deliverytime
            font-size:10px 
            color:rgb(147,153,159)
            line-height:12px
        .text 
          font-size:12px 
          color:rgb(7,17,27) 
          line-height:18px
        .recommend-wrapper 
          .icon-thumb_up 
            font-size:12px 
            line-height:16px
            color:rgb(0,160,220)
          .recommend
            padding: 2px 6px
            margin: 4px
            font-size:9px 
            color:rgb(147,153,159)
            line-height:16px 
            border:1px rgba(7,17,27,0.1) solid
            border-radius:2px 
            display:inline-block
</style>
