<!--
 * @Author: ShawnPhang
 * @Date: 2022-07-19 17:18:35
 * @Description:  
 * @LastEditors: ShawnPhang
 * @LastEditTime: 2022-07-20 14:54:57
 * @site: book.palxp.com
-->
<template>
  <div class="img__box">
    <div v-show="loading" class="load"></div>
    <img v-show="!loading" :src="src" @load="loadDone" />
  </div>
</template>

<script>
export default {
  props: {
    src: {},
  },
  data() {
    return {
      loading: false,
    }
  },
  watch: {
    src() {
      this.loading = true
    },
  },
  methods: {
    loadDone() {
      this.loading = false
    },
  },
}
</script>

<style scoped>
.img__box {
  position: relative;
  overflow: hidden;
}
.img__box > img {
  display: block;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.load {
  position: absolute;
  width: 100%;
  height: 100%;
  /* background: rgba(0, 0, 0, 0.4); */
}
.load::after {
  position: absolute;
  content: '';
  top: 50%;
  left: 50%;
  width: 30px;
  margin-left: -15px;
  height: 30px;
  margin-top: -15px;
  border: 2px solid rgba(255, 255, 255, 0.7);
  border-top-color: transparent;
  border-radius: 100%;
  animation: circle infinite 0.75s linear;
}

@keyframes circle {
  0% {
    transform: rotate(0);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>