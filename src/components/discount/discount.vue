<template>
<div class="discount-container">
    <div class="discount-wrapper">
        <h1 class="name">{{seller.name}}</h1>
        <vStar :score="seller.foodScore" size="24" class="star"></vStar>
        <div class="title">
            <div class="line"></div>
            <div class="text">优惠信息</div>
            <div class="line"></div>
        </div>
        <ul class="discount-list">
            <li v-for="(item,index) in seller.supports" :key="index">
                <span :class="'icon'+item.type" class="icon" alt=""></span>
                <span class="desciption">{{item.description}}</span>
            </li>
        </ul>
        <div class="title">
            <div class="line"></div>
            <div class="text">商家公告</div>
            <div class="line"></div>
        </div>
        <p class="bulletin">{{seller.bulletin}}</p>
    </div>
    <div class="detail-close" @click="hideDetail">
        <i class="icon-close"></i>
    </div>
</div>
    
</template>

<script>
import vStar from '@/components/star/star'

 export default {
     components:{
         vStar
     },
     props:["seller"],
     methods:{
         hideDetail() {
             this.$parent.showDiscount()
         }
     }
 }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
.discount-container
    position:absolute
    top:0 
    left:0
    height:100%
    width:100%
    background:rgba(7,17,27,0.8)
    color:#fff
    z-index:100
    .discount-wrapper
        margin:64px 36px 32px 36px
        height: calc(100% - 96px)
        .star 
            margin-top:16px
            text-align: center
        .name 
            font-size:16px 
            font-weight:700
            line-height:16px
            text-align :center
        .title
            text-align :center
            margin:28px 0 24px 0
            display :flex 
            .line 
                flex:1
                position:relative
                border-bottom:1px solid rgba(255,255,255,0.2)
                top:-6px
            .text 
                font-size:14px 
                font-weight:700
                line-height:14px
                padding:0 12px
        .discount-list 
            margin:24px 0 28px 12px  
            li 
                margin:6px 0
                .icon
                    display:inline-block
                    width:16px
                    height:16px
                    vertical-align:bottom  
                    &.icon0     
                        background:url(./decrease_1@2x.png)  no-repeat  
                        background-size:16px 16px
                    &.icon1,&.icon2
                        background:url(./discount_1@2x.png)  no-repeat  
                        background-size:16px 16px  
                    &.icon3,&.icon4
                        background:url(./special_1@2x.png)  no-repeat  
                        background-size:16px 16px    
                .desciption
                    font-size:12px 
                    line-height:16px
        .bulletin
            font-size:12px 
            line-height:24px
            margin:24px 0 0 12px
            text-align :justify
    .detail-close
        position:relative
        font-size:32px
        text-align :center
        margin:-64px auto 0 auto
</style>