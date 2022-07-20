<!--
 * @Author: ShawnPhang
 * @Date: 2022-07-18 15:49:58
 * @Description:  
 * @LastEditors: ShawnPhang
 * @LastEditTime: 2022-07-20 16:03:55
 * @site: book.palxp.com
-->
<template>
  <div id="app">
    <br /><br />
    <div style="display: flex">
      个数：<input v-model="slidesPerView" type="number" /> 宽度：<input v-model="width" type="text" />
      <button @click="render">点击重绘</button>
    </div>
    <br /><br /><br />

    <div :style="{ width }">
      <my-swiper ref="swiper" style="height: 300px" :list="listData" :slidesPerView="slidesPerView" @change="swiperChange">
        <template #extra>
          <swiper-footer :title="'当前是第 ' + (index + 1) + '张'" @prev="option('prev')" @next="option('next')" />
        </template>
      </my-swiper>
    </div>
  </div>
</template>

<script>
import mySwiper from './components/Swiper.vue'
import swiperFooter from './components/Footer.vue'

export default {
  name: 'App',
  components: {
    mySwiper,
    swiperFooter,
  },
  data() {
    return {
      listData: [],
      index: 1,
      slidesPerView: 3,
      width: '100%',
    }
  },
  created() {
    for (let i = 0; i < 999; i++) {
      this.listData.push({ image: `http://placeimg.com/640/480/any?a=${i}` })
    }
  },
  methods: {
    swiperChange(i) {
      this.index = i
    },
    render() {
      this.$refs.swiper.render()
    },
    option(fn) {
      this.$refs.swiper[fn]()
    },
  },
}
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
