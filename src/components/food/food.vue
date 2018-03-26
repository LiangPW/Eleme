<!-- 商品详情页面组件 -->
<template>
  <transition name="move">
    <div class="food" v-show="showFlag" ref="food">
      <div class="food-content">
        <div class="image-header">
          <img :src="showFood.image" alt="">
          <div class="back" @click="tigger">
            <i class="icon-arrow_lift"></i>
          </div>
        </div>
        <div class="content">
          <h1 class="name">{{showFood.name}}</h1>
          <div class="detail">
            <span class="sell-count">月售{{showFood.sellCount}}份</span>
            <span class="rating">好评率{{showFood.rating}}%</span>
          </div>
          <div class="price">
            <span class="now-price">¥{{showFood.price}}</span>
            <span class="old-price" v-show="showFood.oldPrice">¥{{showFood.oldPrice}}</span>
          </div>
          <div class="carControl-wrapper">
            <carcontrol :food="showFood"></carcontrol>
          </div>
          <div @click="addFirst" class="buy" v-show="(!showFood.count) || (showFood.count === 0)">加入购物车</div>
        </div>
        <split v-show="showFood.info"></split>
        <div class="info" v-show="showFood.info">
          <h1 class="title">商品信息</h1>
          <p class="text">{{showFood.info}}</p>
        </div>
        <split></split>
        <div class="rating">
          <h1 class="title">商品评价</h1>
          <ratingselect v-on:select-text="selectText" v-on:select-type="stype" :ratings="showFood.ratings" :select-type="selectType" :only-content="onlyContent" :desc="desc"></ratingselect>
        </div>
        <div class="rating-wrapper">
          <ul v-show="showFood.ratings && showFood.ratings.length">
            <li v-show="needShow(rating.rateType, rating.text)" v-for="(rating, index) in showFood.ratings" :key="index" class="rating-item">
              <div class="user">
                <span class="name">{{rating.username}}</span>
                <img class="avatar" :src="rating.avatar" width="12" height="12">
              </div>
              <div class="time">{{rating.rateTime | formatDate}}</div>
              <p class="text">
                <span class="icon" :class="{'icon-thumb_up':rating.rateType === 0,'icon-thumb_down':rating.rateType === 1,}"></span>
                {{rating.text}}
              </p>
            </li>
          </ul>
          <div class="on-rating" v-show="!showFood.ratings || !showFood.ratings.length">暂无评价</div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import Vue from 'vue'
import BScoll from 'better-scroll'
import carcontrol from '../carcontrol/carcontrol'
import split from '../split/split'
import ratingselect from '../ratingselect/ratingselect'
import {formatDate} from '@/common/js/data'

// const POSITIVE = 0
// const NEGATIVE = 1
// ALL为默认查看所有评论
const ALL = 2

export default {
  props: {
    showFood: {
      type: Object
    }
  },
  data () {
    return {
      showFlag: false,
      selectType: ALL,
      onlyContent: false,
      desc: {
        all: '全部',
        positive: '推荐',
        negative: '吐槽'
      }
    }
  },
  methods: {
    selectText (data) {
      this.$nextTick(() => {
        this.onlyContent = data
        this.scoll.refresh()
      })
    },
    stype (type) {
      this.$nextTick(() => {
        this.selectType = type
        this.scoll.refresh()
      })
    },
    tigger () {
      this.showFlag = !this.showFlag
      this.$nextTick(() => {
        if (!this.scoll) {
          this.scoll = new BScoll(this.$refs.food, {
            click: true
          })
        } else {
          this.scoll.refresh()
        }
      })
    },
    addFirst (event) {
      if (!event._constructed) {
        return
      }
      Vue.set(this.showFood, 'count', 1)
    },
    needShow (type, text) {
      if (this.onlyContent && !text) {
        return false
      }
      if (this.selectType === ALL) {
        return true
      } else {
        return type === this.selectType
      }
    }
  },
  // 处理时间戳
  filters: {
    formatDate (time) {
      let date = new Date(time)
      return formatDate(date, 'yyyy-MM-dd hh:mm')
    }
  },
  components: {
    'carcontrol': carcontrol,
    'split': split,
    'ratingselect': ratingselect
  }
}
</script>

<style scoped>
.food{
  position: fixed;
  top: 0;
  left: 0;
  bottom: 48px;
  z-index: 30;
  width: 100%;
  background: #fff;
  transition: all .2s linear;
  transform: translate3d(0, 0, 0)
}
.move-enter,.move-leave-active{
  transform: translate3d(100%, 0, 0)
}
.image-header{
  position: relative;
  width: 100%;
  height: 0;
  /* padding-top: 100%这个会根据宽度进行计算 */
  padding-top: 100%;
}
.image-header img{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.back{
  position: absolute;
  top: 10px;
  left: 0;
}
.back i{
  display: inline-block;
  padding: 10px;
  font-size: 20px;
  color: #fff;
}
.content{
  padding: 18px;
  position: relative;
}
.name{
  margin-bottom: 8px;
  line-height: 14px;
  font-size: 14px;
  font-weight: 700;
  color: rgb(7, 17, 27)
}
.detail{
  margin-bottom: 18px;
  line-height: 10px;
  font-size: 0;
}
.sell-count,.rating{
  font-size: 10px;
  color: rgb(147, 153, 159)
}
.sell-count{
  margin-right: 12px;
}
.price{
  font-weight: 700;
  line-height: 24px;
}
.now-price{
  margin-right: 8px;
  font-size: 14px;
  color: rgb(240, 20, 20)
}
.old-price{
  text-decoration: line-through;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.carControl-wrapper{
  position: absolute;
  right: 12px;
  bottom: 12px;
}
.buy{
  position: absolute;
  right: 18px;
  bottom: 18px;
  z-index: 10;
  line-height: 24px;
  height: 24px;
  box-sizing: border-box;
  padding: 0 12px;
  font-size: 10px;
  color: #fff;
  border-radius: 12px;
  background: rgb(0, 160, 220);
}
.info{
  padding: 18px;
}
.info .title{
  line-height: 14px;
  margin-bottom: 6px;
  font-size: 14px;
  color: #07111b;
  font-weight: 700;
}
.info .text{
  line-height: 24px;
  padding: 0 8px;
  font-size: 12px;
  color: #4d555d;
}
.rating{
  padding-top: 18px;
}
.rating .title{
  line-height: 14px;
  margin-left: 18px;
  font-size: 14px;
  color: #07111b;
  font-weight: 700;
}
.rating-wrapper{
  padding: 0 18px;
}
.rating-item{
  position: relative;
  padding: 16px 0;
  border-bottom: 1px solid rgba(7, 17, 27, .1);
}
.user{
  position: absolute;
  top: 16px;
  right: 0;
  line-height: 12px;
  font-size: 0;
}
.name{
  display: inline-block;
  margin-right: 6px;
  vertical-align: top;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.avatar{
  border-radius: 50%;
}
.time{
  margin-bottom: 6px;
  line-height: 12px;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.text{
  line-height: 16px;
  font-size: 12px;
  color: rgb(7, 17, 27)
}
.text .icon{
  margin-right: 4px;
  line-height: 16px;
  font-size: 12px;
}
.icon-thumb_up{
  color: rgb(0, 160, 220)
}
.icon-thumb_down{
  color: rgb(147, 153, 159);
}
.on-rating{
  padding: 16px;
  font-size: 12px;
  color: rgb(147, 153, 159);
}
</style>
