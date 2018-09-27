<!--  -->
<template>
  <div class="treat-box">
    <div class="treat-title1">医院诊疗数排行</div>
    <ul class="treat-number">
     <li v-for="(item,index) in afterMpphdata">
       <div class="icons" :style="{background:item.color}">{{index+1}}</div>
       <h6>{{item.name}}</h6>
       <p>诊疗数<span>{{item.cnt}}</span></p>
     </li>
     
    </ul>
    <div class="treat-title2">专家解答数排行</div>
    <ul class="nums">
     <li v-for="(item,index) in afterExpphdata">
       <div class="num" :style={color:item.color}>{{index+1}}</div>
       <div class="person-img">
         <img :src="item.avatar" alt="">
       </div>
       <div class="name">{{item.name}}</div>
       <div class="total-num">{{item.posts}}</div>
     </li>
    </ul>
  </div>
</template>

<script>
let colorArr = ['#FA4647','#F79D1D','#7FB5F1','#0963B1','#0963B1']
  export default {
    data() {
      return {};
    },
    props:{
        Mpphdata:{ //医院诊疗次数排行
          type:Array,
          default:function(){
            return []
          }
        },
        Expphdata:{  //专家解答数排行
          type:Array,
          default:function(){
            return []
          }
        }
    },
    // components: {},

    computed: {
      afterMpphdata(){
        for(let i = 0;i < this.Mpphdata.length;i++){
          this.Mpphdata[i].color = colorArr[i]
        }
        return this.Mpphdata.splice(0,3)
      },
      afterExpphdata(){
        for(let i = 0;i < this.Expphdata.length;i++){
          this.Expphdata[i].color = colorArr[i]
        }
        return this.Expphdata
      }
    },

    mounted() {
     
    },

    methods: {
    
    },
    watch:{
     
    }
  }

</script>
<style lang='stylus' scoped>
.treat-box
  margin-top 100px
  position relative
  .treat-title1
    font-size 22px
    padding 21px 0 0 22px
    line-height 45px
    text-indent 18px
  .treat-number
    padding-left 25px
    margin-top 41px
    height 307px
    li
      position relative 
      padding-left 55px
      padding-top 8px
      &>.icons 
        position absolute 
        left 0
        top 0   
        width 35px 
        height 35px
        line-height 35px
        font-size 25px
        font-weight bold 
        color #ffffff
        border-radius 100%
        background pink
        font-style italic
        text-indent: 7px;
      & > h6
        font-size 20px
        font-weight 400
        color #FFFFFF
        margin-bottom 22px  
      & > p
        font-size 14px  
        color #7FB5F1
        & > span 
          font-size 24px 
          color #F79D1D
          vertical-align middle
          margin-left 24px     
  .treat-title2
    padding 0 22px
    text-indent 20px
    font-size 22px
    line-height 45px
  .nums
   padding-top 30px
   & > li 
     font-size 0
     padding-left 32px
     margin-bottom 10px
     .num
       font-size 25px
       font-weight bold
       color #FA4647
       display inline-block 
       vertical-align middle
       margin-right 25px
       font-style italic
     .person-img
       display inline-block 
       vertical-align middle 
       width 50px
       height 50px
       border-radius 100%
       background #7FB5F1
       margin 0 20px 0 25px
       overflow hidden
       & > img 
         width 100%
     .name 
       font-size 20px
       color #ffffff
       display inline-block
       vertical-align middle 
       margin-left 20px
       margin-right 35px
     .total-num 
       font-size 20px
       margin-left 35px
       display inline-block
       vertical-align middle 
       color #7FB5F1         
</style>
