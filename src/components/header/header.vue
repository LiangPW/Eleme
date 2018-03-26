<template>
  <div class="header">
    <div class="content-wrapper">
      <div class="avatar">
        <img :src="seller.avatar" width="60" height="60">
      </div>
      <div class="content">
        <div class="title">
          <span class="brand"></span>
          <span class="name">{{seller.name}}</span>
        </div>
        <div class="description">
          {{seller.description}}/{{seller.deliveryTime}}分钟送达
        </div>
        <div v-if="seller.supports" class="support">
          <classMap class="icon" :iconType="seller.supports[0].type"></classMap><span class="text">{{seller.supports[0].description}}</span>
        </div>
      </div>
      <div v-if="seller.supports" class="support-count" @click="detailShow()">
        <span class="count">{{seller.supports.length}}个</span>
        <!-- <span class="icon-keyboard_arrow_right font-icon"></span> -->
      </div>
    </div>
    <div class="bulletin-wrapper" @click="detailShow()">
      <span class="bulletin-title"></span><span class="bulletin-text">{{seller.bulletin}}</span>
      <!-- <i class="icon-keyboard_arrow_right"></i> -->
    </div>
    <div class="background">
      <img :src="seller.avatar" width="100%" height="100%">
    </div>
    <div v-show="showDetail" class="detail">
      <div class="detail-wapper cleatfix">
        <div class="detail-main">
          <h1 class="name">{{seller.name}}</h1>
          <div class="star-wapper">
            <star :size="48" :score="seller.score"></star>
          </div>
          <div class="sum">
            <div class="sum-line"></div>
            <div class="sum-text">优惠信息</div>
            <div class="sum-line"></div>
          </div>
          <ul v-if="seller.supports" class="supports">
            <li v-for="support in seller.supports" :key="support.index" class="support-item">
              <classMap class="icon" :iconType="support.type"></classMap>
              <span class="supports-text">{{support.description}}</span>
            </li>
          </ul>
          <div class="sum">
            <div class="sum-line"></div>
            <div class="sum-text">商家公告</div>
            <div class="sum-line"></div>
          </div>
          <div class="bulletin">
            <p>{{seller.bulletin}}</p>
          </div>
        </div>
      </div>
      <div class="detail-close icon-close" @click="detailHide()"></div>
    </div>
  </div>
</template>

<script>
import Star from '../srat/star'
import classMap from '../classMap/classMap'

export default {
  name: 'vheader',
  // 接收父组件传来的数据
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      showDetail: false
    }
  },
  methods: {
    detailShow () {
      this.showDetail = true
    },
    detailHide () {
      this.showDetail = false
    }
  },
  components: {
    'star': Star,
    'classMap': classMap
  }
}
</script>

<style scoped>
.header{
  position: relative;
  color: #fff;
  background: rgb(7, 17, 27,.5);
  overflow: hidden;
}
.content-wrapper{
  padding: 24px 12px 18px 24px;
  font-size: 0;
  position: relative;
  z-index: 1;
}
.avatar{
  display: inline-block;
  margin-right: 16px;
  vertical-align: top;
}
.avatar img{
  border-radius: 2px;
}
.content{
  display: inline-block;
  font-size: 14px;
}
.title{
  margin: 2px 0 8px;
  line-height: 18px;
}
.title span{
  height: 18px;
  line-height: 18px;
  display: inline-block;
}
.brand{
  width: 30px;
  background: url('./img/brand@2x.png') no-repeat 0 0;
  background-size: 30px 18px;
  vertical-align: top;
}
.title .name{
  margin-left: 6px;
  font-size: 16px;
  font-weight: bold;
}
.description{
  margin-bottom: 6px;
  line-height: 12px;
  font-size: 12px;
}
.support{
  line-height: 12px;
}
.icon{
  width: 12px;
  height: 12px;
  margin-right: 4px;
  background-size: 12px 12px;
  vertical-align: top;
}
.support .decrease{
  background-image: url('./img/decrease_1@2x.png');
}
.support .discount{
  background-image: url('./img/discount_1@2x.png');
}
.support .guarantee{
  background-image: url('./img/guarantee_1@2x.png');
}
.support .invoice{
  background-image: url('./img/invoice_1@2x.png');
}
.support .special{
  background-image: url('./img/special_1@2x.png');
}
.text{
  font-size: 10px;
}
.support-count{
  position: absolute;
  right: 12px;
  bottom: 14px;
  padding: 0 8px;
  height: 24px;
  line-height: 24px;
  border-radius: 14px;
  background: rgba(0, 0, 0, .2)
}
.count{
  font-size: 10px;
  vertical-align: top;
}
.font-icon{
  display: inline-block;
  font-size: 10px;
  margin-left: 2px;
  line-height: 24px;
}
.bulletin-wrapper{
  height: 28px;
  line-height: 28px;
  padding: 0 22px 0 12px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  background: rgba(0, 0, 0, .2);
}
.bulletin-title{
  display: inline-block;
  width: 22px;
  height: 12px;
  background: url('./img/bulletin@2x.png') no-repeat 0 0;
  background-size: 22px 12px;
  vertical-align: top;
  margin-top: 8px;
}
.bulletin-text{
  vertical-align: top;
  margin: 0 4px;
  font-size: 12px;
}
.background{
  position: absolute;
  top: 0;
  left: 0;
  z-index: -1;
  width: 100%;
  height: 100%;
  filter: blur(10px)
}
.detail{
  position: fixed;
  top: 0;
  left: 0;
  z-index: 100;
  width: 100%;
  height: 100%;
  overflow: auto;
  background: rgba(7, 17,27,.8);
  /* backdrop-filter: blur(10px); */
  /* height: 100vh;
  display: flex;
  flex-flow: column; */
}
.detail-wapper{
  min-height: 100%;
  width: 100%;
}
.detail-main{
  padding: 64px 0;
}
.detail-main .name{
  text-align: center;
  line-height: 16px;
  font-size: 16px;
  font-weight: bold;
}
.detail-close{
  position: relative;
  height: 32px;
  width: 32px;
  line-height: 32px;
  margin: -64px auto 0;
  clear: both;
  color: rgba(255,255,255,0.5);
}
.star-wapper{
  margin-top: 18px;
  padding: 2px 0;
  text-align: center;
}
.sum{
  width: 80%;
  display: flex;
  margin: 28px auto 24px;
}
.sum-line{
  flex: 1;
  position: relative;
  border-bottom: 1px solid rgba(255, 255, 255, .2);
  top: -6px;
}
.sum-text{
  padding: 0 12px;
  font-size: 14px;
  font-weight: 700;
}
.supports{
  width: 80%;
  margin: 0 auto;
}
.support-item{
  padding: 0 12px;
  margin-bottom: 12px;
  font-size: 0;
}
.support-item:last-child{
  margin-bottom: 0;
}
.supports-icon{
  width: 16px;
  height: 16px;
  background-size: 16px 16px;
  vertical-align: top;
  margin-right: 6px;
}
.support-item .decrease{
  background-image: url('./img/decrease_2@2x.png');
}
.support-item .discount{
  background-image: url('./img/discount_2@2x.png');
}
.support-item .guarantee{
  background-image: url('./img/guarantee_2@2x.png');
}
.support-item .invoice{
  background-image: url('./img/invoice_2@2x.png');
}
.support-item .special{
  background-image: url('./img/special_2@2x.png');
}
.supports-text{
  font-size: 12px;
  line-height: 16px;
}
.bulletin{
  width: 80%;
  margin: 0 auto;
}
.bulletin p{
  padding: 0 12px;
  font-size: 12px;
  line-height: 24px;
  font-weight: 200;
}
</style>
