<!--
 * @Author: ShawnPhang
 * @Date: 2022-07-18 15:49:58
 * @Description:  
 * @LastEditors: ShawnPhang
 * @LastEditTime: 2022-07-18 17:20:12
 * @site: book.palxp.com
-->
<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
    <div style="width: 750px">
      <my-swiper
        ref="swiper"
        style="height: 300px"
        @change="swiperChange"
        :list="listData"
      >
        <template #extra>
          <swiper-footer
            :title="'当前是第 ' + (index + 1) + '张'"
            @prev="$refs.swiper.prev()"
            @next="$refs.swiper.next()"
          />
        </template>
      </my-swiper>
    </div>
  </div>
</template>

<script>
import mySwiper from "./components/Swiper.vue";
import swiperFooter from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    mySwiper,
    swiperFooter,
  },
  data() {
    return {
      listData: [],
      index: 1,
    };
  },
  created() {
    for (let i = 0; i < 999; i++) {
      this.listData.push({ image: `http://placeimg.com/640/480/any?a=${i}` });
    }
  },
  methods: {
    swiperChange(i) {
      this.index = i;
    },
  },
};
</script>

<style>
#app {
  display: flex;
  justify-content: center;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
