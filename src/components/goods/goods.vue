<template>
  <div class="container">
    <div class="nav" ref="navScroll">
      <ul>
        <li v-for="(item,index) in goods" :key="item.name" class="nav-item"
        :class="{'current': (indexList[index] <= currentIndex) && (indexList[index+1] > currentIndex)}"
        @click="clickList(index)"
        ref="menuList">
          <span class="nav-name">
            <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="content" ref="contentScroll">
      <ul>
        <li v-for="(item,index) in goods" :key="item.name" class="foods-list"
        :class="{'current': indexList[index] === currentIndex}">
          <h1 class="title">{{item.name}}</h1>
          <ul class="food-wrapper">
            <li v-for="food in item.foods" :key="food.name" class="food-body" @click="selectFood(food,$event)">
              <div class="food-img">
                <img :src="food.icon" width="57" height="57">
              </div>
              <div class="food-content">
                <div class="name">{{food.name}}</div>
                <div class="description">{{food.description}}</div>
                <div class="rating">月售{{food.sellCount}}份 好评率{{food.rating}}%</div>
                <div class="price">
                  <span class="extra">￥</span>
                  <span class="num">{{food.price}}</span>
                  <span class="old-price" v-if="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
              <vCount class="count" :food="food" @drop="drop"></vCount>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <vShopCart class="shop-cart" :selectList="selectLists" :minPrice ="seller.minPrice" :deliveryPrice="seller.deliveryPrice" ref="shopcart"></vShopCart>
    <vFood :food="selectedFood" ref="foodCmp"></vFood>
  </div>
</template>

<script>
import axios from 'axios'
import BScroll from 'better-scroll'
import vCount from '@/components/count/count'
import vShopCart from '@/components/shopCart/shopCart'
import vFood from '@/components/food/food'

export default {
  name: 'Goods',
  props:{
    seller:{
      type:Object
    }
  },
  components: {
    vCount,
    vShopCart,
    vFood
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      goods:[],
      scrollY:0, //右侧列表滑动的y轴坐标
      rightLiTops: [], //所有分类头部位置
      indexList: [],
      selectList: [],
      selectedFood: {},
      classMap: []
    }
  },
  created() {
    this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    this.getInfo()
  },
  methods:{
    getInfo() {
      axios.get('/api/goods').then((res)=> {
        this.goods = res.data.data
        var t = 0
        this.indexList = [0]
        for(let i=0;i<this.goods.length;i++) {
          t += this.goods[i].foods.length
          this.indexList.push(t)
        }
      })
    },
    clickList(index){
        let indexItem = this.indexList[index]
        console.log("indexItem:"+indexItem)
        this.scrollY = this.rightLiTops[indexItem];
        this.rightBscroll.scrollTo(0,-this.scrollY,200,)
    },
    _initBScroll() {
      this.leftBscroll = new BScroll(this.$refs.navScroll,{
        click:true
      });
      this.rightBscroll = new BScroll(this.$refs.contentScroll,{
        click:true,
        probeType: 3
      })
      this.rightBscroll.on('scroll',(pos) => {
        this.scrollY = Math.abs(pos.y);
        // console.log(this.scrollY)
      })
    },
    //求出右边列表的高度
    _initRightHeight(){
      let itemArray=[]; //定义一个伪数组
      let top = 0;
      itemArray.push(top)
      //获取右边所有li的礼
      let allList = this.$refs.contentScroll.getElementsByClassName('food-body');
      //allList伪数组转化成真数组
      Array.prototype.slice.call(allList).forEach(li => {
        top += li.clientHeight; //获取所有li的每一个高度
        itemArray.push(top)
      });
      this.rightLiTops = itemArray;
      //  console.log(this.rightLiTops)
    },
    _initLeftScroll(index){
      let menu = this.$refs.menuList;
      let el = menu[index];
      this.leftBscroll.scrollToElement(el,300,0,-300)
    },
    selectFood(food, event) {
      if (!event._constructed) {
          return;
        }
        this.selectedFood = food;
        this.$refs.foodCmp.show();
    },
    drop(target) {
      // 体验优化,异步执行下落动画
      this.$nextTick(() => {
        this.$refs.shopcart.drop(target);
      });
    },
  },
  computed: {
    //动态绑定class类名
     currentIndex(index) {
      const {scrollY,rightLiTops} = this;
      return rightLiTops.findIndex((tops,index )=>{
        this._initLeftScroll(index);  //调用左右联调滚动效果
        return scrollY >= tops && scrollY < rightLiTops[index + 1]
      })
     },
     selectLists() {
        let foods = [];
        this.goods.forEach((good) => {
          good.foods.forEach((food) => {
            if (food.count) {
              foods.push(food);
            }
          });
        });
        return foods;
    }
  },
  watch:{
    goods(){
      //监听数据
      this.$nextTick(() =>{
        //左右两边滚动
        this. _initBScroll();
        //右边列表高度
        this._initRightHeight()
      })
    }
  },
}
</script>

<style lang='stylus' rel='stylesheet/stylus'>
@import "../../common/stylus/mixin.styl"

.container
  display: flex
  position: absolute
  top: 179px
  bottom:0
  width: 100%
  overflow: hidden
  .nav
    flex:0 0 80px
    background: #f3f5f7
    .nav-item
      display: table;
      height: 54px;
      width: 56px;
      padding: 0 12px;
      line-height: 14px;
      &.current
        background:#fff
      .nav-name
        display: table-cell
        width: 56px
        vertical-align: middle
        position: relative
        font-size: 12px
        border-bottom:1px rgba(7,17,27,0.1) solid
        .icon
          display: inline-block
          vertical-align: top
          width: 12px
          height: 12px
          margin-right: 2px
          background-size: 12px 12px
          background-repeat: no-repeat
          &.decrease
            bg-image('decrease_3')
          &.discount
            bg-image('discount_3')
          &.guarantee
            bg-image('guarantee_3')
          &.invoice
            bg-image('invoice_3')
          &.special
            bg-image('special_3')
  .content
    flex:1
    .foods-list
      .title
        font-size:12px 
        color:rgb(147,153,159)
        line-height: 26px
        padding-left:14px 
        border-left:4px #d9dde1 solid
        background:#f3f5f7
      .food-wrapper
        padding: 0 18px
        .food-body
          padding:18px 0
          display:flex
          border-bottom: 1px rgba(7,17,27,0.1) solid
          position:relative
          &:last-child
            border-bottom:none
          .food-img
            flex:0 0 57px
          .food-content
            color:rgb(147,153,159)
            flex:1
            margin-left:10px
            .name 
              font-size:14px 
              color:rgb(7,17,27)
              line-height: 14px
              font-weight:700
              margin: 2px 0 8px 0
            .description
              font-size:10px 
              line-height: 14px
              margin-bottom:8px
            .rating 
              font-size:10px 
              line-height: 10px
            .price
              font-size:10px 
              color:rgb(147,153,159)
              line-height: 24px
              font-weight:700
              .extra
                color:rgb(240,20,20)
              .num 
                font-size:14px 
                color:rgb(240,20,20)
                font-weight:700
              .old-price
                text-decoration: line-through
        .count
          position:absolute
          right:0 
          bottom:18px
  .shop-cart
    position:absolute 
    bottom:0
    left:0
    width:100%
</style>
