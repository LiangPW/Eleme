<template>
  <div class="star" :class="sratType">
    <span v-for="itemClass in itemClasses" :key="itemClass.index" :class="itemClass" class="star-item"></span>
  </div>
</template>

<script>
const LENGTH = 5
const CLS_ON = 'on'
const CLS_HALF = 'half'
const CLS_OFF = 'off'

export default {
  name: 'Star',
  props: {
    size: {
      type: Number
    },
    score: {
      type: Number
    }
  },
  computed: {
    sratType () {
      return 'star-' + this.size
    },
    itemClasses () {
      let result = []
      let score = Math.floor(this.score * 2) / 2
      let halfStar = score % 1 !== 0
      let intStar = Math.floor(score)
      for (let i = 0; i < intStar; i++) {
        result.push(CLS_ON)
      }
      if (halfStar) {
        result.push(CLS_HALF)
      }
      while (result.length < LENGTH) {
        result.push(CLS_OFF)
      }
      return result
    }
  }
}
</script>

<style scoped>
.star {
  font-size: 0;
}
.star-48 .star-item {
  width: 20px;
  height: 20px;
  margin-right: 22px;
  background-size: 20px 20px;
}
.star-48 .on {
  background-image: url("./img/star48_on@2x.png");
}
.star-48 .half {
  background-image: url("./img/star48_half@2x.png");
}
.star-48 .off {
  background-image: url("./img/star48_off@2x.png");
}
.star-36 .star-item {
  width: 15px;
  height: 15px;
  margin-right: 6px;
  background-size: 15px 15px;
}
.star-36 .on {
  background-image: url("./img/star36_on@2x.png");
}
.star-36 .half {
  background-image: url("./img/star36_half@2x.png");
}
.star-36 .off {
  background-image: url("./img/star36_off@2x.png");
}
.star-24 .star-item {
  width: 10px;
  height: 10px;
  margin-right: 3px;
  background-size: 10px 10px;
}
.star-24 .on {
  background-image: url("./img/star24_on@2x.png");
}
.star-24 .half {
  background-image: url("./img/star24_half@2x.png");
}
.star-24 .off {
  background-image: url("./img/star24_off@2x.png");
}

.star-item {
  background-repeat: no-repeat;
  display: inline-block;
}
.star-item:last-child {
  margin-right: 0;
}
</style>
