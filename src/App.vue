<!--
 * @Author: ShawnPhang
 * @Date: 2022-07-18 15:49:58
 * @Description:  
 * @LastEditors: ShawnPhang
 * @LastEditTime: 2022-08-15 17:21:58
 * @site: book.palxp.com
-->
<template>
  <div id="app">
    <br /><br />
    <div style="display: flex">展示个数：<input v-model="slidesPerView" type="number" />；列表总数：<input v-model="listNum" type="number" />；宽度：<input v-model="width" type="text" /></div>
    <br /><br /><br />

    <div id="demo" :style="{ width, height: '350px' }">
      <my-swiper ref="swiper" style="height: 300px" :list="listData" :slidesPerView="slidesPerView" @change="swiperChange">
        <template #extra><swiper-footer :title="'当前是第 ' + (index + 1) + '张'" @prev="option('prev')" @next="option('next')" /></template>
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
      listNum: 999,
      index: 1,
      slidesPerView: 3,
      width: '60%',
    }
  },
  watch: {
    listNum() {
      this.createList()
    },
  },
  created() {
    this.createList()
  },
  async mounted() {
    await this.$nextTick()
    const myObserver = new ResizeObserver((entries) => {
      entries.forEach((entry) => {
        this.render()
      })
    })
    myObserver.observe(document.querySelector('#demo'))
  },
  methods: {
    createList() {
      this.listData.length = 0
      this.index = 1
      for (let i = 0; i < this.listNum; i++) {
        this.listData.push({ image: `http://placeimg.com/640/480/any?a=${i}` })
      }
    },
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
#demo {
  position: relative;
}
</style>
