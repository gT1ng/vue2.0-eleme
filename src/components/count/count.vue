<template>
 <div class='count-wrapper'>
     <transition name="move">
        <div class="cart-decrease" v-show="food.count>0" @click.stop.prevent="increaseNum">
            <span class="inner icon-remove_circle_outline"></span>
        </div>
    </transition>
     <span class="num" v-show="food.count">{{food.count}}</span>
     <span class="cart-add icon-add_circle" @click.stop.prevent="addNum"></span>
 </div>
</template>

<script>
import Vue from 'vue'

 export default {
     props:{
        food:{
            type:Object
        }
     },
     data() {
         return {
             count:0
         }
     },
     methods: {
         addNum(event) {
             if(!this.food.count) {
                this.$set(this.food,'count',1)
             }else{
                this.food.count++
             }
             this.$emit('drop', event.target)
         },
         increaseNum() {
             this.food.count--
         }
     }
 }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
.count-wrapper
    font-size:24px
    line-height:24px
    color: rgb(0,160,220)
    .num
        font-size:10px 
        color:rgb(147,153,159)
        line-height:24px 
        vertical-align: top
    .cart-decrease
        display: inline-block
        transition: all 0.4s linear
        .inner
            display: inline-block
            line-height: 24px
            font-size: 24px
            color: rgb(0, 160, 220)
            transition: all 0.4s linear
            transform: rotate(0)
        &.move-enter,&.move-leave-to
            transform:translateX(24px)
            .inner
                transform: rotate(180deg)
        &.move-enter-active,&.move-leave-active
            opacity:1
            transition:all .5s linear
            .inner
                transition:all .5s linear
</style>