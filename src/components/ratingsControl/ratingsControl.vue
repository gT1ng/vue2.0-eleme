<template>
 <div class='control-wrapper'>
     <div class="type-wrapper">
        <span class="rating-type positive" :class="{'active':selectType==2}" @click="select(2)">
            全部
            <span class="num">{{all}}</span>
        </span>
        <span class="rating-type positive" :class="{'active':selectType==0}" @click="select(0)">
            推荐
            <span class="num">{{positive}}</span>
        </span>
        <span class="rating-type nagetive" :class="{'active':selectType==1}" @click="select(1)">
            吐槽
            <span class="num">{{nagetive}}</span>
        </span>
     </div>
     <div class="switch" :class="{'active':onlyContent}" @click="ifContent">
         <span class="icon-check_circle"></span>
         <span class="text">只看有内容的评论</span>
     </div>
 </div>
</template>

<script>
 export default {
     props:{
         ratings: {
             type: Array,
             default() {
                return [];
            }
         }
    },
     data() {
         return {
             selectType: 2,
             onlyContent: false
         }
     },
     computed: {
         all() {
             let count = 0
             this.ratings.forEach(rating => {
                 count++
             });
             return count
         },
         positive() {
             let count = 0
             this.ratings.forEach(rating => {
                if(rating.rateType==1) count++
             });
             return count
         },
         nagetive() {
             let count = 0
             this.ratings.forEach(rating => {
                 if(rating.rateType==0) count++
             });
             return count
         }
     },
     methods: {
         ifContent(event) {
            this.onlyContent = !this.onlyContent;
            this.$emit('ifContent', this.onlyContent); // 提交名为'ifContent'的事件给父组件;
         },
         select(type) {
             this.selectType = type;
             this.$emit('selectType', this.selectType);
         }
     }
 }
</script>

<style lang='stylus' rel='stylesheet/stylus'>
.control-wrapper
    .type-wrapper
        padding: 12px 0 18px 0
        font-size: 0
        .rating-type
            padding: 8px
            font-size: 12px
            line-height: 16px
            display: inline-block
            border-radius: 2px
            margin: 0 4px
            .num 
                font-size: 8px
            &.positive 
                color: rgb(77, 85, 93) 
                background: rgba(0, 160, 220, 0.2)
                &.active 
                    color: #fff 
                    background: rgb(0, 160, 220)
            &.nagetive 
                color: rgb(77, 85, 93) 
                background: rgba(77, 85, 93, 0.2)
                &.active 
                    color: #fff 
                    background: #4d555d;
    .switch
        padding: 12px 0
        border-top: 1px rgba(7 17, 27, 0.1) solid
        .icon-check_circle
            font-size: 24px 
            color: rgb(147, 153, 159)
            line-height: 24px
        .text
            display: inline-block
            font-size: 12px 
            color: rgb(147, 153, 159)
            line-height: 24px
            vertical-align: top
        &.active
            .icon-check_circle
                color:#00c850
</style>