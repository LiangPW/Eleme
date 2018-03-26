<template>
  <div class="ratings" ref="ratingScroll">
    <div class="ratingScroll">
      <div class="ratings-content">
        <div class="overview">
          <div class="overview-left">
            <h1 class="score">{{seller.score}}</h1>
            <div class="title">综合评分</div>
            <div class="rank">高于周边商家{{seller.rankRate}}%</div>
          </div>
          <div class="overview-right">
            <div class="score-wrapper">
              <span class="title">服务态度:</span>
              <star class="star" :size="36" :score="seller.serviceScore"></star>
              <span class="score">{{seller.serviceScore}}</span>
            </div>
            <div class="score-wrapper">
              <span class="title">商品评分:</span>
              <star class="star" :size="36" :score="seller.foodScore"></star>
              <span class="score">{{seller.foodScore}}</span>
            </div>
            <div class="delivery-wrapper">
              <span class="title">送达时间:</span>
              <span class="time">{{seller.deliveryTime}}分钟</span>
            </div>
          </div>
        </div>
      </div>
      <split></split>
      <ratingselect v-on:select-text="selectText" v-on:select-type="stype" :ratings="Ratings" :select-type="selectType" :only-content="onlyContent"></ratingselect>
      <div class="rating-wrapper">
        <ul>
          <li class="rating-item" v-show="needShow(rating.rateType, rating.text)" v-for="(rating, index) in Ratings" :key="index">
            <div class="avatar">
              <img :src="rating.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star class="star" :size="24" :score="rating.score"></star>
                <span v-show="rating.deliveryTime" class="deliveryTime">{{rating.deliveryTime}}分钟送达</span>
              </div>
              <p class="text">
                {{rating.text}}
              </p>
              <div class="goods" v-show="rating.recommend">
                <span class="icon" :class="{'icon-thumb_up':rating.rateType === 0,'icon-thumb_down':rating.rateType === 1,}"></span>
                <span class="recommend" v-for="(item, index) in rating.recommend" :key="index">{{item}}</span>
              </div>
              <div class="time">{{rating.rateTime | formatDate}}</div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import split from '../split/split'
import star from '../srat/star'
import BScoll from 'better-scroll'
import ratingselect from '../ratingselect/ratingselect'
import {formatDate} from '@/common/js/data'

// const POSITIVE = 0
// const NEGATIVE = 1
// ALL为默认查看所有评论
const ALL = 2
const ERR_OK = 0

export default {
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      selectType: ALL,
      onlyContent: false,
      Ratings: []
    }
  },
  created () {
    this.$http.get('/api/ratings').then((res) => {
      if (res.data.errno === ERR_OK) {
        this.Ratings = res.data.data
        this.$nextTick(() => {
          this.scroll = new BScoll(this.$refs.ratingScroll, {
            click: true
          })
        })
      }
    })
  },
  methods: {
    selectText (data) {
      this.$nextTick(() => {
        this.onlyContent = data
        this.scroll.refresh()
      })
    },
    stype (type) {
      this.$nextTick(() => {
        this.selectType = type
        this.scroll.refresh()
      })
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
    'split': split,
    'star': star,
    'ratingselect': ratingselect
  }
}
</script>

<style scoped>
.ratings {
  position: absolute;
  top: 171px;
  bottom: 0;
  left: 0;
  width: 100%;
  overflow: hidden;
}
.overview{
  display: flex;
  padding: 18px 0;
}
.overview-left{
  flex: 0 0 127px;
  width: 127px;
  border-right: 1px solid rgba(7, 17, 27, .1);
  text-align: center;
  padding: 6px 0;
}
.overview-left .score{
  margin-bottom: 6px;
  line-height: 28px;
  font-size: 24px;
  color: rgb(255, 153, 0);
}
.overview-left .title{
  margin-bottom: 8px;
  line-height: 12px;
  font-size: 12px;
  color: rgb(7, 17, 27);
}
.overview-left .rank{
  line-height: 10px;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.overview-right{
  flex: 1;
  padding: 6px 0 6px 24px;
}
.score-wrapper{
  margin-bottom: 8px;
  line-height: 18px;
  font-size: 0;
}
.score-wrapper .title,.delivery-wrapper .title{
  display: inline-block;
  line-height: 18px;
  vertical-align: top;
  font-size: 12px;
  color: rgb(7, 17, 27);
}
.score-wrapper .star{
  display: inline-block;
  margin: 0 12px;
  vertical-align: top;
}
.score-wrapper .score{
  display: inline-block;
  vertical-align: top;
  line-height: 18px;
  font-size: 12px;
  color: rgb(255, 153, 0);
}
.delivery-wrapper{
  font-size: 0;
}
.delivery-wrapper .time{
  margin-left: 12px;
  line-height: 18px;
  font-size: 12px;
  color: rgb(147, 153, 159);
}
.rating-wrapper{
  padding: 0 18px;
}
.rating-item{
  position: relative;
  display: flex;
  padding: 18px 0;
  border-bottom: 1px solid rgba(147, 153, 159, .1);
}
.avatar{
  flex: 0 0 28px;
  margin-right: 12px;
  width: 28px;
}
.avatar img{
  width: 28px;
  height: 28px;
  border-radius: 50%;
}
.content{
  flex: 1;
}
.name{
  line-height: 12px;
  margin-bottom: 4px;
  font-size: 10px;
  color: rgb(7, 17, 27);
}
.star-wrapper{
  margin-bottom: 6px;
  font-size: 0;
}
.star-wrapper .star{
  display: inline-block;
  line-height: 12px;
  margin-right: 6px;
}
.deliveryTime{
  display: inline-block;
  line-height: 12px;
  font-weight: 200;
  font-size: 10px;
  color: rgb(147, 153, 159);
}
.text{
  line-height: 18px;
  font-size: 12px;
  margin-bottom: 8px;
  color: rgb(7, 17, 27);
}
.goods{
  font-size: 0;
  height: 19px;
  overflow: hidden;
}
.icon{
  line-height: 18px;
  font-size: 12px;
}
.icon-thumb_up{
  color: rgb(0, 160, 220)
}
.icon-thumb_down{
  color: rgb(147, 153, 159);
}
.recommend{
  display: inline-block;
  margin-left: 8px;
  padding: 0 6px;
  line-height: 16px;
  font-size: 9px;
  border: 1px solid rgba(147, 153, 159, .1);
  color: rgb(147, 153, 159);
  border-radius: 2px;
}
.rating-item .time{
  position: absolute;
  top: 18px;
  right: 0;
  line-height: 12px;
  font-size: 10px;
  font-weight: 200;
  color: rgb(147, 153, 159);
}
@media only screen and (max-width: 320px) {
  .overview-left{
    flex: 0 0 110px;
    width: 110px;
  }
  .overview-right{
    padding-left: 6px;
  }
}
</style>
