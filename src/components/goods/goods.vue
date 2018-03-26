<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuWrapper">
      <ul class="menu-ul">
        <li v-for="(item, index) in goods" :key="item.index" class="menu-li" :class="{'current':currentIndex === index}" v-on:click="seleceMenu(index,$event)">
          <span class="menu-text"><classMap v-show="item.type>0" class="menu-icon" :iconType="item.type"></classMap>{{item.name}}</span>
        </li>
      </ul>
    </div>
    <div class="goods-wrapper" ref="foodsWrapper">
      <ul class="foods-ul">
        <li v-for="good in goods" :key="good.index" class="good-item food-list-hook">
          <h1 class="good-title">{{good.name}}</h1>
          <ul>
            <li v-for="food in good.foods" :key="food.index" class="food-item" @click="selectFood(food,$event)">
              <div class="food-img">
                <img width="57" :src="food.icon" alt="">
              </div>
              <div class="food-content">
                <h2 class="food-name">{{food.name}}</h2>
                <p class="food-desc">{{food.description}}</p>
                <div class="food-extra">
                  <span>月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="food-price">
                  <span class="now-price">¥{{food.price}}</span><span class="old-price" v-show="food.oldPrice">¥{{food.oldPrice}}</span>
                </div>
              </div>
              <div class="carControl-wrapper">
                <carcontrol :food="food" v-on:cart-add="cartAdd"></carcontrol>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopCar ref="shopcar" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopCar>
    <food :show-food="selectedFood" ref="food"></food>
  </div>
</template>

<script>
import classMap from '../classMap/classMap'
import BScoll from 'better-scroll'
import shopCar from '../shopcar/shopcar'
import carcontrol from '../carcontrol/carcontrol'
import food from '../food/food'

const ERR_OK = 0

export default {
  name: 'Goods',
  props: {
    seller: {
      type: Object
    }
  },
  data () {
    return {
      goods: [],
      listHeight: [],
      scollY: 0,
      selectedFood: {}
    }
  },
  computed: {
    currentIndex () {
      for (let i = 0; i < this.listHeight.length; i++) {
        let height1 = this.listHeight[i]
        let height2 = this.listHeight[(i + 1)]
        if (!height2 || ((this.scollY >= height1) && (this.scollY < height2))) {
          if (this.scollY < this.listHeight[1]) {
            return 0
          }
          return i
        }
      }
    },
    selectFoods () {
      let carfoods = []
      this.goods.forEach((good) => {
        good.foods.forEach((food) => {
          if (food.count) {
            carfoods.push(food)
          }
        })
      })
      return carfoods
    }
  },
  created () {
    this.$http.get('/api/goods').then((res) => {
      if (res.data.errno === ERR_OK) {
        this.goods = res.data.data
        this.$nextTick(() => {
          this._initScoll()
          this._calculateHeight()
        })
      }
    })
  },
  methods: {
    _initScoll () {
      this.menuScoll = new BScoll(this.$refs.menuWrapper, {
        click: true
      })
      this.foodsScoll = new BScoll(this.$refs.foodsWrapper, {
        probeType: 3,
        click: true
      })
      this.foodsScoll.on('scroll', (pos) => {
        this.scollY = Math.abs(Math.round(pos.y))
      })
    },
    _calculateHeight () {
      let foodsList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
      let height = 0
      this.listHeight.push(height)
      for (let i = 0; i < foodsList.length; i++) {
        height = height + foodsList[i].clientHeight
        this.listHeight.push(height)
      }
    },
    seleceMenu (index, event) {
      if (!event._constructed) {
        return
      }
      let foodList = this.$refs.foodsWrapper.getElementsByClassName('food-list-hook')
      let el = foodList[index]
      this.foodsScoll.scrollToElement(el, 300)
    },
    _drop (target) {
      // 优化体验，异步执行下落动画
      this.$nextTick(() => {
        this.$refs.shopcar.drop(target)
      })
    },
    cartAdd (target) {
      this._drop(target)
    },
    selectFood (food, event) {
      if (!event._constructed) {
        return
      }
      this.selectedFood = food
      this.$refs.food.tigger()
    }
  },
  components: {
    'classMap': classMap,
    'shopCar': shopCar,
    'carcontrol': carcontrol,
    'food': food
  }
}
</script>

<style scoped>
.goods {
  display: flex;
  position: absolute;
  top: 171px;
  bottom: 46px;
  width: 100%;
  overflow: hidden;
}
.menu-wrapper{
  flex: 0 0 80px;
  width: 80px;
  background: #f3f5f7;
}
.menu-li{
  display: table;
  width: 56px;
  height: 54px;
  line-height: 14px;
  padding: 0 12px;
}
.menu-icon{
  width: 12px;
  height: 12px;
  margin-right: 2px;
  background-size: 12px 12px;
  vertical-align: top;
}
.menu-li .decrease{
  background-image: url('./img/decrease_3@2x.png');
}
.menu-li .discount{
  background-image: url('./img/discount_3@2x.png');
}
.menu-li .guarantee{
  background-image: url('./img/guarantee_3@2x.png');
}
.menu-li .invoice{
  background-image: url('./img/invoice_3@2x.png');
}
.menu-li .special{
  background-image: url('./img/special_3@2x.png');
}
.menu-text{
  display: table-cell;
  vertical-align: middle;
  width: 56px;
  font-size: 12px;
  font-weight: 200;
  border-bottom: 1px solid rgba(7,17,27,.1);
}
.goods-wrapper{
  flex: 1;
}
.good-title{
  padding-left: 14px;
  height: 26px;
  line-height: 26px;
  font-size: 12px;
  background: #f3f5f7;
  border-left: 2px solid #d9dde1;
  color: rgba(147, 153, 159, 1);
}
.good-item{
}
.food-item{
  position: relative;
  display: flex;
  margin: 18px 18px 0 18px;
  padding-bottom: 18px;
  border-bottom: 1px solid rgba(7,17,27,.1);
}
.food-item:last-child{
  border-bottom: 0 none;
}
.food-img{
  flex: 0 0 57px;
  margin-right: 10px;
  line-height: 0;
}
.food-content{
  flex: 1;
}
.food-name{
  margin: 2px 0 8px;
  height: 14px;
  line-height: 14px;
  font-size: 14px;
  color: rgb(7, 17, 27)
}
.food-desc,.food-extra{
  line-height: 10px;
  font-size: 10px;
  color: rgb(147, 153, 159)
}
.food-desc{
  margin-bottom: 8px;
  line-height: 12px;
}
.food-extra span:first-child{
  margin-right: 8px;
}
.food-price{
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
.current{
  background: #fff;
  margin-top: -1px;
  border-bottom: none;
  font-weight: 700;
}
.carControl-wrapper{
  position: absolute;
  bottom: 12px;
  right: -6px;
  z-index: 10;
}
</style>
