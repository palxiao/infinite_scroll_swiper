<!--
 * @Author: ShawnPhang
 * @Date: 2022-07-15 15:48:31
 * @Description:  
 * @LastEditors: ShawnPhang
 * @LastEditTime: 2022-07-19 17:30:48
 * @site: book.palxp.com
-->
<template>
  <div class="foot">
    <a class="left" @click="$emit('prev')"><div class="triangle"></div></a>
    <span class="title">{{ title }}</span>
    <a class="right" @click="$emit('next')"><div class="triangle"></div></a>
  </div>
</template>

<script>
export default {
  props: {
    title: {},
  },
};
</script>

<style scoped>
.foot {
  position: absolute;
  bottom: 0;
  width: 100%;
  display: flex;
  justify-content: center;
}
.title {
  opacity: 0.6;
  width: 24%;
  margin: 0 2%;
  text-align: center;
  background: #17398e;
  border: 1px solid #ffffff;
  padding: 6px 45px;
  font-size: 20px;
  font-family: SourceHanSansCN-Bold, SourceHanSansCN;
  font-weight: bold;
  color: #dbe6ff;
  line-height: 29px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  position: relative;
}
.triangle {
  width: 0;
  height: 0;
  border: 9px solid;
  border-left-width: 0;
  border-right-width: 18px;
  border-color: transparent #6e99fc transparent transparent;
}
.left,
.right {
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 42px;
  height: 42px;
  background: linear-gradient(
    315deg,
    rgba(23, 57, 142, 0.1) 0%,
    rgba(73, 111, 208, 0.4) 100%
  );
  border: 1px solid;
  border-image: linear-gradient(
      90deg,
      rgba(170, 195, 255, 1),
      rgba(255, 255, 255, 0)
    )
    1 1;
}
/* .left:hover, .right:hover {

} */
.right {
  transform: rotate(180deg);
}
</style>