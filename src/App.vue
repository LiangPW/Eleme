<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <router-link to="/">商品</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/ratings">评论</router-link>
      </div>
      <div class="tab-item">
        <router-link to="/seller">商家</router-link>
      </div>
    </div>
    <div class="content">
      <keep-alive>
        <router-view :seller="seller"></router-view>
      </keep-alive>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import vheader from './components/header/header.vue'

const ERR_OK = 0

export default {
  name: 'App',
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.$http.get('/api/seller').then((res) => {
      if (res.data.errno === ERR_OK) {
        this.seller = res.data.data
      }
    })
  },
  components: {
    'v-header': vheader
  }
}
</script>

<style>
.tab {
  width: 100%;
  height: 40px;
  line-height: 40px;
  display: flex;
  border-bottom: 1px solid rgba(7, 17, 27, 0.1);
}
/* .tab:after{} */
.tab-item {
  flex: 1;
  text-align: center;
}
.tab-item a {
  display: block;
  font-size: 14px;
  color: rgba(77, 85, 93, 1);
}
.tab-item a.router-link-exact-active {
  color: brown;
}
</style>
