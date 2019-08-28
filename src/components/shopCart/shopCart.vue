<template>
<div>
    <div class='cart-container'>
        <transition name="move">
            <div class="cart-list" v-show="ifShow">
                <div class="head">
                    <span class="title">购物车</span>
                    <span class="clear" @click="clear">清空</span>
                </div>
                <ul class="list-wrapper">
                    <li v-for="(item, index) in selectList" :key = "index" v-show="item.count>0">
                        <span class="name">{{item.name}}</span>
                        <vCount class="count" :food="item"></vCount>
                        <span class="price">￥<span class="num">{{item.price}}</span></span>
                    </li>
                </ul>
            </div>
        </transition>
        
        <div class="ball-container">
            <transition name="drop" @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter" v-for="(ball,index) in balls" :key="index" >
                <div v-show="ball.show" class="ball">
                    <div class="inner inner-hook"></div>
                </div>
            </transition>
        </div>
        <div class="cart-wrapper">
            <div class="left" @click="showShopList">
                <div class="icon" :class="{'highlight':totalPrice>0}">
                    <i class="icon-shopping_cart"></i>
                    <div class="count" v-show="totalCount">{{totalCount}}</div>
                </div>
                <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
                <div class="delivery-price">另需配送费￥{{deliveryPrice}}元</div>
            </div>
            <div class="right min-price" :class="{'enough': enough}" @click="pay">
                {{payDesc}}
            </div>
        </div>
    </div>
    <transition name="fade">
        <div class="black" v-show="ifShow" @click="hideShopList"></div>
    </transition>
</div>

</template>

<script>
import axios from 'axios'
import vCount from '@/components/count/count'

 export default {
     props:{
         selectList: {
             type: Array
         },
         minPrice: {
             type: Number,
             default: 0
         },
         deliveryPrice: {
             type: Number,
             default: 0
         }
     },
     data() {
         return {
             seller:{},
             ifShow:false,
             enough:false,
             balls: [
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                }
            ],
            dropBalls: [],
            fold: true
         }
     },
     components: {
         vCount
     },
     created() {
     },
     methods: {
        showShopList() {
            if (this.totalCount === 0) return
            this.ifShow = !this.ifShow
        },
        hideShopList() {
            if (this.totalCount === 0) return
            this.ifShow = false
        },
        pay() {
            if(this.totalPrice < this.minPrice) return
            alert(`支付${this.totalPrice}元`)
        },
        clear() {
            this.selectList.forEach((food) => {
                food.count = 0;
            });
            this.ifShow = false
        },
        drop(el) {
            console.log("drop")
            for (let i = 0; i < this.balls.length; i++) {
                let ball = this.balls[i];
                if (!ball.show) {
                    ball.show = true;
                    ball.el = el;
                    this.dropBalls.push(ball);
                    return;
                }
            }
        },
        beforeEnter(el) {
          let count = this.balls.length;
          while (count--) {
            let ball = this.balls[count];
            if (ball.show) {
              let rect = ball.el.getBoundingClientRect();
              let x = rect.left - 32;
              let y = -(window.innerHeight - rect.top - 22);
              el.style.display = '';
              el.style.webkitTransform = `translate3d(0,${y}px,0)`;
              el.style.transform = `translate3d(0,${y}px,0)`;
              let inner = el.getElementsByClassName('inner-hook')[0];
              inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
              inner.style.transform = `translate3d(${x}px,0,0)`;
            }
          }
        },
        enter(el) {
          /* eslint-disable no-unused-vars */
          let rf = el.offsetHeight;
          this.$nextTick(() => {
            el.style.webkitTransform = 'translate3d(0,0,0)';
            el.style.transform = 'translate3d(0,0,0)';
            let inner = el.getElementsByClassName('inner-hook')[0];
            inner.style.webkitTransform = 'translate3d(0,0,0)';
            inner.style.transform = 'translate3d(0,0,0)';
          });
        },
        afterEnter(el) {
          let ball = this.dropBalls.shift();
          if (ball) {
            ball.show = false;
            el.style.display = 'none';
          }
        }
     },
     computed: {
         totalPrice() {
            let total = 0;
            this.selectList.forEach((food)=>{
                total += food.count*food.price
            })
            return total
         },
         totalCount() {
             let count = 0;
             this.selectList.forEach((food)=>{
                count += food.count
            })
            if(count === 0) this.ifShow = false
            return count
         },
         payDesc() {
             if(this.totalPrice === 0) {
                 this.enough = false
                 return `￥${this.minPrice}起送`
             } else if(this.totalPrice < this.minPrice) {
                 this.enough = false
                 let differ = this.minPrice - this.totalPrice
                 return `还差￥${differ}起送`
             } else {
                 this.enough = true
                 return `去结算`
             }
         }
     },
     transitions: {
      drop: {
        beforeEnter(el) {
          let count = this.balls.length;
          while (count--) {
            let ball = this.balls[count];
            if (ball.show) {
              let rect = ball.el.getBoundingClientRect();
              let x = rect.left - 32;
              let y = -(window.innerHeight - rect.top - 22);
              el.style.display = '';
              el.style.webkitTransform = `translate3d(0,${y}px,0)`;
              el.style.transform = `translate3d(0,${y}px,0)`;
              let inner = el.getElementsByClassName('inner-hook')[0];
              inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
              inner.style.transform = `translate3d(${x}px,0,0)`;
            }
          }
        },
        enter(el) {
          /* eslint-disable no-unused-vars */
          let rf = el.offsetHeight;
          this.$nextTick(() => {
            el.style.webkitTransform = 'translate3d(0,0,0)';
            el.style.transform = 'translate3d(0,0,0)';
            let inner = el.getElementsByClassName('inner-hook')[0];
            inner.style.webkitTransform = 'translate3d(0,0,0)';
            inner.style.transform = 'translate3d(0,0,0)';
          });
        },
        afterEnter(el) {
          let ball = this.dropBalls.shift();
          if (ball) {
            ball.show = false;
            el.style.display = 'none';
          }
        }
      }
    }
 }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
.cart-container
    position :fixed
    bottom:0 
    left:0  
    width:100%
    height:48px 
    z-index:50
    .cart-wrapper
        height:48px 
        width:100%
        background:#141d27
        color:rgba(255,255,255,0.4)
        display:flex 
        z-index:11
        .left 
            flex:1
            padding:12px 0
            .icon
                height:44px 
                width:44px 
                margin:-23px 18px 8px 18px
                background:#2b333b
                border-radius:50%
                border:6px #141d27 solid
                display:inline-block
                position: absolute
                text-align: center
                &.highlight
                    background:rgb(0,160,220)
                i 
                    font-size:24px 
                    color:rgb(255,255,255,0.4)
                    line-height: 44px
                .count
                    position: absolute
                    top: -5px
                    right: -10px
                    width: 25px
                    border-radius: 8px
                    font-size:9px 
                    font-weight:700
                    color:#fff 
                    line-height:16px
                    background:rgb(240,20,20)
                    box-shadow: 0px 4px 8px 0px rgba(0,0,0,0.4)
            .price
                display:inline-block
                font-size:16px 
                font-weight:700 
                line-height:24px
                vertical-align:top
                padding:0 12px 0 80px
                &.highlight
                    color:#fff
            .delivery-price
                display:inline-block
                padding-left:12px 
                border-left:1px rgba(255,255,255,0.1) solid
                font-size:12px 
                line-height:24px
                vertical-align:top
        .right 
            flex:0 0 105px
            background:#2b333b
            &.min-price
                font-size:12px 
                font-weight:700 
                line-height:48px
                text-align:center
                &.enough 
                    background: #00b43c
                    color: #fff
    .cart-list
        z-index:-1
        transition:all 0.7s linear
        position: absolute;
        /* top: 0; */
        width: 100%;
        left: 0;
        bottom: 48px;
        &.move-enter, &.move-leave-active
            transform: translate3d(0, 100%, 0)
        .head
            background:#f3f5f7
            border-bottom:1px rgba(7,17,27,0.1) solid
            .title 
                font-size:14px 
                color:rgb(7,17,27)
                line-height:40px
                padding:0 18px
            .clear 
                font-size:12px 
                color:rgb(0,160,220)
                line-height:40px
                padding:0 18px
                float:right
        .list-wrapper
            background:#fff
            li 
                padding:12px 18px
                .name 
                    font-size:14px 
                    color:rgb(7,17,27)
                    line-height:24px
                    display:inline-block
                .count 
                    float:right
                    width:70px
                    text-align: right
                .price 
                    font-size:10px 
                    color:rgb(240,20,20)
                    line-height:24px
                    display:inline-block
                    padding:0 12px
                    float:right
                    .num 
                        font-size:14px 
                        font-weight:700
    .ball-container
        .ball
            position: fixed
            left: 38px
            bottom: 22px
            z-index: 200
            transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
            .inner
                width: 16px
                height: 16px
                border-radius: 50%
                background: rgb(0, 160, 220)
                transition: all 0.4s linear
.black
    position:fixed 
    height:100% 
    width:100% 
    top:0 
    left:0 
    background:rgba(7,17,27,0.6)
    backdrop-filter: blur(10px)
    transition:all 0.7s linear;
    z-index:10
    /*** 开始插入、移除结束的位置变化 ***/
    &.fade-enter, &.fade-leave-active
      opacity: 0
</style>