<template>
 <div class=''>
     <div class="star-wrapper" :class="starClass">
         <span  v-for="(item,index) in starList" :key="index" class="star" :class="item"></span>
     </div>
 </div>
</template>

<script>
 export default {
     props:{
         score:{
             type:Number
         },
         size: {
             type:Number
         }
     },
     computed: {
         grayStar() {
             return parseInt(5 - this.score)
         },
         yellowStar() {
             return parseInt(this.score)
         },
         halfStar() {
             if(this.grayStar + this.yellowStar === 5) return 0
             else return 1
         },
         starList() {
             let list = []
             for(let i=0; i<this.yellowStar; i++) {
                 list.push("yellowStar")
             }
             for(let i=0; i<this.halfStar; i++) {
                 list.push("halfStar")
             }
             for(let i=0; i<this.grayStar; i++) {
                 list.push("grayStar")
             }
             return list
         },
         starClass() {
             return 'star-' + this.size
         }
     }
 }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
    .star-24
        .star
            display:inline-block
            width:24px 
            height:24px
            margin:0 12px
            &.yellowStar
                background: url(./star36_on@2x.png)
                background-size: 24px 24px
            &.halfStar 
                background: url(./star36_half@2x.png)
                background-size: 24px 24px
            &.grayStar 
                background: url(./star36_off@2x.png)
                background-size: 24px 24px
    .star-12
        .star
            display:inline-block
            width:12px 
            height:12px
            margin:0 4px
            &.yellowStar
                background: url(./star36_on@2x.png)
                background-size: 12px 12px
            &.halfStar 
                background: url(./star36_half@2x.png)
                background-size: 12px 12px
            &.grayStar 
                background: url(./star36_off@2x.png)
                background-size: 12px 12px
    .star-10
        .star
            display:inline-block
            width:10px 
            height:10px
            margin:0 2px
            &.yellowStar
                background: url(./star36_on@2x.png)
                background-size: 10px 10px
            &.halfStar 
                background: url(./star36_half@2x.png)
                background-size: 10px 10px
            &.grayStar 
                background: url(./star36_off@2x.png)
                background-size: 10px 10px
</style>