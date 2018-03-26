<!-- 评论功能组件 -->
<template>
  <div class="ratingSelect">
    <div class="rating-type">
      <span @click="select(2,$event)" class="block positive" :class="{'active':selectType === 2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
      <span @click="select(0,$event)" class="block positive" :class="{'active':selectType === 0}">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
      <span @click="select(1,$event)" class="block negative" :class="{'active1':selectType === 1}">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
    </div>
    <div class="switch" @click.stop="toggleContent" :class="{'on':onlyContent}">
      <span class="icon-check_circle icon"></span>
      <span class="text">只看有评论的内容</span>
    </div>
  </div>
</template>

<script>
const POSITIVE = 0
const NEGATIVE = 1
const ALL = 2

export default {
  data () {
    return {

    }
  },
  props: {
    ratings: {
      type: Array,
      default () {
        return []
      }
    },
    selectType: {
      type: Number,
      default: ALL
    },
    onlyContent: {
      type: Boolean,
      default: false
    },
    desc: {
      type: Object,
      default () {
        return {
          all: '全部',
          positive: '满意',
          negative: '不满意'
        }
      }
    }
  },
  computed: {
    positives () {
      let positivesArr = []
      this.ratings.forEach((rating) => {
        if (rating.rateType === POSITIVE) {
          positivesArr.push(rating)
        }
      })
      return positivesArr
    },
    negatives () {
      let pnegativesArr = []
      this.ratings.forEach((rating) => {
        if (rating.rateType === NEGATIVE) {
          pnegativesArr.push(rating)
        }
      })
      return pnegativesArr
    }
  },
  methods: {
    select (type, event) {
      if (!event._constructed) {
        return
      }
      this.selectType = type
      this.$emit('select-type', this.selectType)
    },
    toggleContent (event) {
      if (!event._constructed) {
        return
      }
      this.onlyContent = !this.onlyContent
      this.$emit('select-text', this.onlyContent)
    }
  }
}
</script>

<style scoped>
.ratingSelect{}
.rating-type{
  padding: 18px 0;
  margin: 0 18px;
  border-bottom: 1px solid rgba(7, 17, 27, .2);
  font-size: 0;
}
.block{
  display: inline-block;
  padding: 8px 12px;
  margin-right: 8px;
  border-radius: 1px;
  font-size: 12px;
  line-height: 16px;
  color: rgb(77, 85, 93);
}
.positive{
  background: rgba(0, 160 , 220, .1);
}
.negative{
  background: rgba(77, 85, 93, .2);
}
.count{
  font-size: 8px;
  margin-left: 2px;
  vertical-align: bottom;
}
.active{
  background: rgba(0, 160 , 220, 1);
  color: #fff;
}
.active1{
  color: #fff;
  background: rgba(77, 85, 93, 1);
}
.switch{
  padding: 12px 18px;
  line-height: 24px;
  border-bottom: 1px solid rgba(7, 17, 27, .1);
  color: rgb(147, 153, 159);
  font-size: 0;
  display: flex;
}
.switch .icon{
  display: inline-block;
  font-size: 24px;
  margin-right: 4px;
}
.on .icon{
  color: rgb(1, 238, 159);
}
.switch .text{
  font-size: 12px;
}
</style>
