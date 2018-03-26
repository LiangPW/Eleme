<!-- 添加/删除商品控件 -->
<template>
  <div class="carcontrol">
    <transition name="move">
      <div class="car-decrease" v-show="food.count > 0" v-on:click="removeCount">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="car-count" v-show="food.count > 0">{{food.count}}</div>
    <div class="car-add icon-add_circle" v-on:click="addCount"></div>
  </div>
</template>

<script>
import Vue from 'vue'

export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCount (event) {
      event.cancelBubble = true
      if (!event._constructed) {
        return
      }
      if (!this.food.count) {
        Vue.set(this.food, 'count', 1)
      } else {
        this.food.count++
      }
      this.$emit('cart-add', event.target)
    },
    removeCount (event) {
      // 阻止事件冒泡
      event.cancelBubble = true
      if (!event._constructed) {
        return
      }
      this.food.count--
    }
  }
}
</script>

<style scoped>
.car-decrease div{
  display: inline-block;
}
.car-decrease,.car-add{
  display: inline-block;
  padding: 6px;
}
.car-decrease{
  transition: all .4s linear;
  transform: translate3d(0, 0, 0);
  opacity: 1;
}

.move-enter,.move-leave-active{
  opacity: 0;
  transform: translate3d(24px, 0, 0);
}
.move-enter .inner,.move-leave-active .inner{
  transform: rotate(180deg)
}
.inner{
  display: inline-block;
  line-height: 24px;
  font-size: 24px;
  color: rgb(0, 160, 220);
  transition: all .4s linear;
  transform: rotate(0)
}
.car-add{
  line-height: 24px;
  font-size: 24px;
  color: rgb(0, 160, 220)
}
.car-count{
  display: inline-block;
  vertical-align: top;
  width: 12px;
  padding-top: 6;
  line-height: 24px;
  text-align: center;
  font-size: 12px;
  color: rgb(147, 153, 159)
}
.ball-wrapper{
}
.ball{
  position: fixed;
  left: 32px;
  bottom: 32px;
  z-index:200;
}
.ball .inner{
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: rgb(0, 160, 220);
  transition: all .4s;
}
.drop-transition{
  transition: all .4s;
}
/* @keyframes {} */
</style>
