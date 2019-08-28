<template>
<div>
    <div class='header'>
        <div class="container-wrapper">
            <div class="avatar">
                <img :src="seller.avatar" alt="">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div class="sale">
                    <span :class="'icon'+type" class="icon" alt=""></span>
                    <span class="price">{{description}}</span>
                </div>
            </div>
            <div class="supports" @click="showDiscount">5个 ></div>
        </div>
        <div class="notice">
            <span class='icon'></span>
            <span class='text'>{{seller.bulletin}}</span>
            <span class="more" @click="showDiscount">></span>
        </div>
        <div class="background">
            <img :src="seller.avatar" width="100%" height="100%">
        </div>
    </div>
    <discount :seller="seller" v-if="ifShowDiscount"></discount>
</div>
</template>

<script>
    import axios from 'axios'
    import discount from '@/components/discount/discount'

    export default {
        components:{
            discount
        },
        data() {
            return {
                seller:{},
                description:'',
                type: 0,
                ifShowDiscount:false
            }
        },
        created() {
            this.getSeller()
        },
        mounted() {
            
        },
        methods: {
            getSeller () {
                axios.get('/api/seller').then((res)=>{
                    this.seller = res.data.data
                    this.description = res.data.data.supports[0].description
                    this.type = res.data.data.supports[0].type
                })
            },
            showDiscount() {
                this.ifShowDiscount =!this.ifShowDiscount
            }
        }
    }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
.header
    position:relative
    width:100%
    overflow:hidden
    background: rgba(7, 17, 27, 0.5)
    color:rgb(255,255,255)
    .container-wrapper
        padding:24px 12px 18px 24px
        .avatar
            display:inline-block 
            img 
                width:64px
                height:64px
                border-radius:4px
        .content
            display:inline-block
            margin:2px 16px 0 16px
            vertical-align: top
            .title
                margin-bottom:8px 
                .brand
                    display :inline-block
                    width:30px 
                    height:18px
                    background:url(./brand@2x.png) no-repeat
                    background-size:30px 18px
                    vertical-align:top
                .name
                    font-size:16px
                    font-weight:700
                    line-height:18px
            .description
                font-size:12px
                margin-bottom:8px 
                font-weight:200 
                line-height:12px
            .sale
                .price
                    font-size:10px 
                    font-weight:200 
                    line-height: 12px
                .icon
                    display:inline-block
                    width:14px
                    height:14px
                    vertical-align:bottom  
                    &.icon0     
                        background:url(./decrease_1@2x.png)  no-repeat  
                        background-size:14px 14px 
                        vertical-align: middle 
        .supports
            position:absolute
            right: 12px
            bottom: 42px
            font-size:10px 
            line-height:12px 
            padding:7px 8px 
            border-radius:14px 
            background:rgba(0,0,0,0.2)
    .notice
        position: relative
        height: 28px
        line-height: 28px
        padding: 0 22px 0 12px
        white-space: nowrap
        overflow: hidden
        text-overflow: ellipsis
        background: rgba(7, 17, 27, 0.2)
        .icon 
            display: inline-block
            vertical-align: top
            margin-top: 8px
            width: 22px
            height: 12px
            background:url(./bulletin@2x.png) no-repeat
            background-size: 22px 12px
        .text 
            vertical-align: top
            margin: 0 4px
            font-size: 10px
        .more 
            position: absolute
            font-size: 10px
            right: 12px
    .background
        position:absolute
        width:100%
        height:100% 
        top:0 
        left:0
        z-index:-1 
        filter:blur(10px)
</style>