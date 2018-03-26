<template>
  <div class="seller" ref="sellerScroll">
   <div class="seller-content">
     <div class="overview">
       <h1 class="title">{{seller.name}}</h1>
       <div class="desc">
         <star class="star" :size="36" :score="seller.score"></star>
         <span class="text">({{seller.ratingCount}})</span>
         <span class="text">月售{{seller.sellCount}}单</span>
       </div>
       <ul class="remark">
         <li class="block">
           <h2>起送价</h2>
           <div class="content">
             <span class="stress">{{seller.minPrice}}</span>元
           </div>
         </li>
         <li class="block">
           <h2>商家配送</h2>
           <div class="content">
             <span class="stress">{{seller.deliveryPrice}}</span>元
           </div>
         </li>
         <li class="block">
           <h2>平均配送时间</h2>
           <div class="content">
             <span class="stress">{{seller.deliveryTime}}</span>分钟
           </div>
         </li>
       </ul>
       <div class="favorite" @click="favoriteClick">
         <span class="icon-favorite" :class="{'favoriteOn':favorite}"></span>
         <span class="text">{{favoriteText}}</span>
       </div>
     </div>
     <split></split>
     <div class="bulletin">
       <h1 class="title">公告与活动</h1>
       <div class="content-wrapper">
         <p class="content">{{seller.bulletin}}</p>
       </div>
       <ul v-if="seller.supports" class="supports">
          <li v-for="support in seller.supports" :key="support.index" class="support-item">
            <classMap class="icon" :iconType="support.type"></classMap>
            <span class="supports-text">{{support.description}}</span>
          </li>
        </ul>
     </div>
     <split></split>
     <div class="pics">
       <h1 class="title">商家实景</h1>
       <div class="pic-wrapper" ref="picWrapper">
         <ul class="pic-list" ref="picList">
           <li class="pic-item" v-for="(pic, index) in seller.pics" :key="index">
             <img width="120" height="90" :src="pic" alt="">
           </li>
         </ul>
       </div>
     </div>
     <split></split>
     <div class="info">
       <h1 class="infoTitle">商家信息</h1>
       <ul>
         <li class="info-item" v-for="(info, index) in seller.infos" :key="index">{{info}}</li>
       </ul>
     </div>
   </div>
  </div>
</template>

<script>
import split from '../split/split'
import star from '../srat/star'
import BScoll from 'better-scroll'
import classMap from '../classMap/classMap'

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      favorite: false,
      favoriteText: '收藏'
    }
  },
  mounted () {
    this._sellerScroll()
    this._picScroll()
  },
  watch: {
    'seller' () {
      this._sellerScroll()
      this._picScroll()
    }
  },
  methods: {
    _sellerScroll () {
      this.$nextTick(() => {
        if (!this.scroll) {
          this.scroll = new BScoll(this.$refs.sellerScroll, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      })
    },
    _picScroll () {
      if (this.seller.pics) {
        let picWidth = 120
        let picMargin = 6
        let allPicWidth = (picWidth + picMargin) * this.seller.pics.length - picMargin
        this.$refs.picList.style.width = allPicWidth
        this.$nextTick(() => {
          if (!this.picScroll) {
            this.picScroll = new BScoll(this.$refs.picWrapper, {
              scrollX: true,
              scrollY: false
            })
          } else {
            this.picScroll.refresh()
          }
        })
      }
    },
    favoriteClick () {
      this.favorite = !this.favorite
      this.favoriteText = this.favorite ? '已收藏' : '收藏'
    }
  },
  components: {
    'split': split,
    'star': star,
    'classMap': classMap
  }
}
</script>

<style scoped>
.seller {
  position: absolute;
  top: 171px;
  bottom: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
}
.overview{
  padding: 18px;
  position: relative;
}
.title{
  margin-bottom: 8px;
  line-height: 14px;
  color: rgb(7, 17, 27);
  font-size: 14px;
}
.desc{
  padding-bottom: 18px;
  line-height: 18px;
  border-bottom: 1px solid rgba(7,17,27,.1);
  font-size: 0;
}
.star{
  display: inline-block;
  margin-right: 8px;
  vertical-align: top;
}
.desc .text{
  display: inline-block;
  margin-right: 12px;
  vertical-align: top;
  font-size: 10px;
  color: rgb(77, 85, 93)
}
.remark{
  display: flex;
  padding-top: 18px;
}
.block{
  flex: 1;
  text-align: center;
  border-right: 1px solid rgba(7,17,27,.1);
}
.block:last-child{
  border-right: 0 none;
}
.block h2{
  margin-bottom: 4px;
  line-height: 10px;
  font-size: 10px;
  color: rgb(147, 153, 159)
}
.block .content{
  line-height: 24px;
  height: 24px;
  font-size: 10px;
  color: rgb(7, 17, 27);
}
.stress{
  font-size: 24px;
  font-weight: 300;
  line-height: 24px;
}
.favorite{
  position: absolute;
  top: 18px;
  right: 11px;
  text-align: center;
  width: 50px;
}
.icon-favorite{
  display: block;
  margin-bottom: 4px;
  line-height: 24px;
  color: #d4d6d9;
  font-size: 24px;
}
.favoriteOn{
  color: rgb(240, 20, 20);
}
.favorite .text{
  line-height: 10px;
  font-size: 10px;

}
.bulletin{
  padding: 18px 18px 0 18px;
}
.content-wrapper{
  padding: 0 12px 16px 12px;
}
.content{
  line-height: 24px;
  color: rgb(240, 20, 20);
  font-size: 12px;
  font-weight: 300;
}
.supports{}
.support-item{
  padding: 16px 12px;
  font-size: 0;
  border-top: 1px solid rgba(7, 17, 27, .1);
}
.icon{
  width: 16px;
  height: 16px;
  margin-right: 6px;
  background-size: 16px 16px;
  vertical-align: top;
}
.supports .decrease{
  background-image: url('./img/decrease_4@2x.png');
}
.supports .discount{
  background-image: url('./img/discount_4@2x.png');
}
.supports .guarantee{
  background-image: url('./img/guarantee_4@2x.png');
}
.supports .invoice{
  background-image: url('./img/invoice_4@2x.png');
}
.supports .special{
  background-image: url('./img/special_4@2x.png');
}
.supports-text{
  line-height: 16px;
  font-size: 12px;
  font-weight: 300;
  color: rgb(7, 17, 27);
}
.pics{
  padding: 18px;
}
.pics .title{
  margin-bottom: 12px;
}
.pic-wrapper{
  width: 100%;
  white-space:nowrap;
  overflow: hidden;
}
.pic-list{
  font-size: 0;
}
.pic-item{
  display: inline-block;
  margin-right: 6px;
  width: 120px;
  height: 90px;
}
.pic-item:last-child{
  margin-right: 0;
}
.info{
  padding: 18px 18px 0 18px;
  color: rgb(7, 17, 27)
}
.infoTitle{
  padding-bottom: 12px;
  line-height: 14px;
  font-size: 14px;
  border-bottom: 1px solid rgba(7, 17, 27, .1);
}
.info-item{
  padding: 16px 12px;
  line-height: 12px;
  border-bottom: 1px solid rgba(7, 17, 27, .1);
  font-size: 12px;
}
</style>
