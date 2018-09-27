<!--  -->
<template>
  <div class="city-box">
    <div class="city-top clearfix">
      <div class="city-image" id="map" :style="{width:'545px',height:'575px'}">
        <!-- <img src="./city.png" alt=""> -->
      </div>
      <div class="city-data">
        <div class="city-number">
          <div class="item">
            <span>{{Mpubcount}}</span>
            <p>庄稼医院( 所 )</p>
          </div>
          <div class="item">
            <span>{{Usercount}}</span>
            <p>会员( 个 )</p>
          </div>
          <div class="item">
            <span>{{Expertcount}}</span>
            <p>专家( 位 )</p>
          </div>
          <div class="item">
            <span>{{Totalview.total}}</span>
            <p>诊疗( 次 )</p>
          </div>
        </div>
      </div>
      <div class="introduce-text">
        {{Description}}
      </div>
      <div class="introduce-img">
        <div class="swiper-container">
          <div class="swiper-wrapper">
            <div class="introduce-img-wrap swiper-slide" v-for="(item,index) in Hotmpdata">
              <div class="title">{{item.name}}</div>
              <p>
                <span>位 置：</span>{{item.position}}
              </p>
              <p>
                <span>诊疗科室：</span>{{item.keshi}}
              </p>
              <div class="swpier-box">
                <div class="swiper-container2">
                  <div class="swiper-wrapper">
                    <div class="swiper-slide" v-for="(item,index) in item.pic">
                      <img :src="item.url" alt="">
                      <div class="swiper-title">{{item.alt}}</div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>
    </div>
    <div class="city-bottom clearfix">
      <a @click="goHref" class="tv-href ">进入视频诊疗室</a>
      <a @click="goHref" class="city-href"><img src="./city-href.png" alt=""></a>
    </div>
  </div>
</template>

<script>
let echarts = require('echarts/lib/echarts')
require('echarts/lib/chart/pie')
require('echarts/lib/chart/bar')
require('echarts/lib/chart/map')
require('echarts/lib/component/tooltip')
require('echarts/lib/component/title')
import zj from 'echarts/map/json/province/zhejiang'
import Swiper from 'swiper'

export default {
  data () {
    return {
      showFlag:0
    };
  },
  props:{
    Mpubcount:String,
    Usercount:Number,
    Expertcount:String,
    Fstusername:String,
    FstRoomid:String,
    Fstuserpw:String,
    cityActive:{
      type:Number,
      default:0
    },
    provinceList:{
      type:Array,
      default:function(){
        return []
      }
    },
    Description:String, // 医院介绍文案
    Totalview:Object, //诊疗次数数据 
    Hotmpdata:{  //专科医院介绍+诊疗视频图片
      type:Array,
      default:function(){
        return []
      }
    }
  },
  // components: {},
  computed:{
    cityList(){
      let arr = []
      for(let i = 0;i<this.provinceList.length;i++){
        arr.push(this.provinceList[i].name)
      }
      return arr.reverse();
    },
    cityNum(){
       let arr = []
      for(let i = 0;i<this.provinceList.length;i++){     
        arr.push(this.provinceList[i].nums)
      }
      return arr.reverse();
    },
    barGapArr(){
      let arr = []
      let length = this.cityNum.length
      for(let i =0;i<length;i++){
        arr.push(this.cityNum[length - 1])
      }
      return arr
    },
    cityData(){
      let cityName = this.cityList.concat([]).splice(0,this.cityList.length - 1).reverse()
      let num = this.cityNum.concat([]).splice(0,this.cityNum.length - 1).reverse()
      let arr = []
      for(let i = 0;i<cityName.length;i++){
        arr.push({name:`${cityName[i]}市`,value:num[i],selected:false})
      }
      arr[this.cityActive].selected = true
      // arr[this.cityActive].value = this.cityActive
      return arr
    },
    hosImg(){
      let arr = []
      for(let i = 0;i < this.Hotmpdata.length;i++){
        arr.push(this.Hotmpdata[i].pic)
      }
      return arr
    }
  },
  methods: {
    goHref:function() {
      var ua = navigator.userAgent.toLowerCase();
      if (ua.indexOf(" chrome/") >= 0 || ua.indexOf(" firefox/") >= 0 || ua.indexOf(' gecko/') >= 0) {
        var StringMaker = function () {
          this.str = "";
          this.length = 0;
          this.append = function (s) {
            this.str += s;
            this.length += s.length;
          }
          this.prepend = function (s) {
            this.str = s + this.str;
            this.length += s.length;
          }
          this.toString = function () {
            return this.str;
          }
        }
      } else {
        var StringMaker = function () {
          this.parts = [];
          this.length = 0;
          this.append = function (s) {
            this.parts.push(s);
            this.length += s.length;
          }
          this.prepend = function (s) {
            this.parts.unshift(s);
            this.length += s.length;
          }
          this.toString = function () {
            return this.parts.join('');
          }
        }
      }


      var keyStr = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";

      function encode64(input) {
        var output = new StringMaker();
        var chr1, chr2, chr3;
        var enc1, enc2, enc3, enc4;
        var i = 0;

        while (i < input.length) {
          chr1 = input.charCodeAt(i++);
          chr2 = input.charCodeAt(i++);
          chr3 = input.charCodeAt(i++);

          enc1 = chr1 >> 2;
          enc2 = ((chr1 & 3) << 4) | (chr2 >> 4);
          enc3 = ((chr2 & 15) << 2) | (chr3 >> 6);
          enc4 = chr3 & 63;

          if (isNaN(chr2)) {
            enc3 = enc4 = 64;
          } else if (isNaN(chr3)) {
            enc4 = 64;
          }

          output.append(keyStr.charAt(enc1) + keyStr.charAt(enc2) + keyStr.charAt(enc3) + keyStr.charAt(enc4));
        }

        return output.toString();
      }

      let str = `{http://47.97.61.230:8080/download/ZNZX_for_Windows_V3.13.5.8.exe}{Fastonz}{FMDesktop}{-link TCP:47.97.61.230:1089 -uname ${this.Fstusername} -upwd ${this.Fstuserpw} -utype 1 -rid ${this.FstRoomid} -stype 1}`
      str = encodeURIComponent(str)
      let url = "launcher.fsm://" + encode64(str)
      console.log(str)
      
      window.location.href = url
      
    },
    initMap(){
      let that = this
      // 注册可用的地图
      let mapEcharts = echarts.init(document.getElementById('map'))
      echarts.registerMap('zhejiang', zj);
      mapEcharts.setOption({
                    tooltip: {
                        trigger: 'item',
                        axisPointer: {
                            type: 'shadow',
                            shadowWidth:'60%'
                        },
                        formatter: '{b}: {c}所医院'
                    },
                    series: [
                        {
                          name: '浙江地图',
                          type: 'map',
                          map: 'zhejiang',
                          mapType: 'zj',
                          roam: false,
                          zoom: 1.2,
                          selectedMode: 'single', 
                          itemStyle:{
                              normal:{
                                  areaColor: '#226DBF',
                                  label:{
                                      show:true,
                                      textStyle: {
                                          color: '#fff',
                                          fontSize: 16
                                      }
                                  },
                              },
                              emphasis:{
                                  areaColor: '#F79D1D',
                                  label:{
                                      show:true,
                                      textStyle: {
                                          color: '#000',
                                          fontSize:16
                                      }
                                  }
                                    
                              }
                            },
                          data:this.cityData
                        }
                    ]
                },true); 
          mapEcharts.off('click') 
          mapEcharts.on('click',function(params){
            that.$emit('mapFn',params.name,params.dataIndex)
          
          })         
      }
  },

  mounted(){
    this.initMap()
    new Swiper('.swiper-container', {
          autoplay: {
            delay: 18000,
            stopOnLastSlide: false,
            disableOnInteraction: true
          },
          loop : true
        })
    new Swiper('.swiper-container2', {
      autoplay: {
        delay: 3000,
        stopOnLastSlide: false,
        disableOnInteraction: true
      },
      loop : true
    })
  },
  watch:{
    cityActive(){
      this.initMap()
    },
    
  },
  // created() {
  // },
  // components:{
   
  // }
  // methods: {}
}

</script>
<style lang='stylus' scoped>
.city-box
  padding-top 100px
  .city-top
    padding-top 63px
    padding-bottom 6px
    .city-image
      width 545px
      height 575px
      margin-left 100px
      margin-right 95px
      float left
      & > img
        width 100%
        height 100%
    .city-data
      width 516px
      // margin-right 60px
      margin-bottom 25px
      position relative
      float left
      .city-number
        height 80px
        & > .item
          width 25%
          float left
          text-align center
          & > span
            font-size 30px
            color #f79d1d
            line-height 30px
            margin 13px 0 8px
            display block
          & > p
            font-size 14px
            color #7fb5f1
        &:after
          content ''
          position absolute
          top 0
          left 0
          width 100%
          height 2px
          background url('./border-longest.png') no-repeat
        &:before
          content ''
          position absolute
          bottom 0
          left 0
          width 100%
          height 2px
          background url('./border-longest.png') no-repeat
        .data-box
          width 415px
          height 445px
    .introduce-text
      width 510px
      height 150px
      background url('./introduce-text.png') no-repeat
      margin-top 25px
      overflow hidden
      margin-bottom 10px
      float left
      padding 16px 20px
      font-size 12px
      font-family SimSun
      font-weight 400
      color #FFFFFF
      line-height 20px
    .introduce-img
      width 510px
      height 220px
      float left
      margin-top 10px
      background url('./introduce-img.png') no-repeat
      padding 20px 22px
      overflow hidden
      .swiper-container
        height 220px
        width auto
      .introduce-img-wrap
        height 200px
        width 510px
        position relative
        display inline-block
      .title
        font-size 14px
        font-family SimSun
        font-weight 400
        color #F79D1D
        line-height 1
        margin-bottom 20px
      p
        position relative
        padding-left 60px
        width 203px
        text-align left
        line-height 18px
        margin-bottom 3px
        font-family SimSun
        & > span
          position absolute
          left 0
          width 60px
          display inline-block
          text-align justify
          &:after
            width 100%
            height 0
            margin 0
            display inline-block
            overflow hidden
            content ''
      .swpier-box
        position absolute
        right 13px
        bottom 45px
        width 200px
        height 155px
        .swiper-container2
          width 100%
          height 200px
          overflow hidden
          img
            width 100%
            height 155px
        .swiper-title
          text-align center
          margin-top 15px
  .city-bottom
    margin-top 17px
    height 164px
    .city-href
      width 647px
      height 126px
      float left
      margin-top 23px
      margin-left 62px
      cursor pointer
      & > img
        display block
        width 100%
        height 100%
    .tv-href
      float left
      display block
      color #F79D1D
      width 215px
      height 55px
      border 1px solid #F79D1D
      font-size 20px
      line-height 55px
      background url('./icon-02.png') no-repeat
      background-position 181px center
      text-align left
      padding-left 30px
      margin 54px 60px 0 120px
      cursor pointer
</style>