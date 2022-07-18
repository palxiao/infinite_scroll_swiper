<!--
 * @Author: ShawnPhang
 * @Date: 2022-07-13 15:33:44
 * @Description:  
 * @LastEditors: ShawnPhang
 * @LastEditTime: 2022-07-18 18:36:39
 * @site: book.palxp.com
-->
<template>
  <div ref="swiper" v-show="width" :style="{ width }" class="swiper">
    <div class="ul">
      <img
        :style="{ width: `${itemWidth}px`, marginRight: `${spaceBetween}px` }"
        :class="[
          'li',
          { 'nth-main': i === initialSlide },
          { 'nth-left': i < initialSlide },
          { 'nth-right': i > initialSlide },
        ]"
        v-for="(item, i) in realImages"
        :key="i + 'i'"
        :src="item.image"
      />
    </div>
    <slot name="extra" />
  </div>
</template>

<script>
export default {
  props: {
    list: {
      default: () => [],
    },
  },
  data() {
    return {
      width: 0,
      spaceBetween: 48, // 图片之间的间距
      itemWidth: 0,
      slidesPerView: 3, // 一屏幕显示几个? 数字或者默认  auto 自动。
      initialSlide: 1, // 从第几个开始
      curIndex: 0,
      transTime: 1000, // 过渡时间
      images: [],
      swiperOption: {
        autoplay: {
          delay: 3000,
        },
      },
    };
  },
  watch: {
    list() {
      this.images = this.list;
    },
  },
  computed: {
    realImages() {
      return this.images.slice(0, 4);
    },
  },
  async mounted() {
    this.images = this.list;
    this.curIndex = this.initialSlide;
    this.count = 0;
    await this.$nextTick();
    const dom = this.$refs.swiper;
    const father = dom.parentElement || dom.parentNode;
    this.width = father.offsetWidth + "px";
    this.itemWidth =
      (father.offsetWidth - this.spaceBetween * (this.slidesPerView - 1)) / 3;
  },
  methods: {
    domOperation(transition, left) {
      this._ulDom = this._ulDom || document.getElementsByClassName("ul")[0];
      this._ulDom.style.transition = transition;
      this._ulDom.style.left = left;
    },
    prev() {
      if (this._running) {
        return;
      }
      this._running = true;
      this.count = this.curIndex;
      this.curIndex =
        this.count - 1 < 0 ? this.images.length - 1 : this.count - 1;
      this.images.unshift(this.images[this.images.length - 1]);
      this.domOperation("0s", `-${this.itemWidth + this.spaceBetween}px`);
      setTimeout(() => {
        this.domOperation(`${this.transTime / 1000}s`, "0px");
        setTimeout(() => {
          this.images.pop();
          this.done();
        }, this.transTime);
      }, 10);
    },
    next() {
      if (this._running) {
        return;
      }
      this._running = true;
      clearTimeout(this.timer1);
      this.count++;
      this.initialSlide++;
      this.domOperation(
        `${this.transTime / 1000}s`,
        `-${this.itemWidth * this.count + this.spaceBetween}px`
      );
      this.timer1 = setTimeout(() => {
        if (this.count) {
          let newIndex = (this.curIndex + this.count) % this.images.length;
          newIndex === this.images.length && (newIndex = 0);
          this.curIndex = newIndex;

          const step = this.count % this.images.length;
          for (let i = 0; i < step; i++) {
            this.images.push(this.images.shift());
          }

          this.initialSlide--;
          this.domOperation("0s", "0px");
          this.done();
        }
      }, this.transTime);
    },
    done() {
      this.count = 0;
      this._running = false;
      this.$emit("change", this.curIndex);
    },
  },
};
</script>

<style scoped>
.swiper {
  position: relative;
  height: 100%;
  margin: 0 auto;
  overflow: hidden;
}
.ul {
  position: absolute;
  display: flex;
  width: 4200px;
  height: 100%;
  left: 0;
  padding: 0;
  margin: 0;
}
.li {
  list-style: none;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 30px;
  font-weight: 800;
  background: #001b57;
  /* box-shadow: 0px 6px 20px 0px rgba(0, 79, 255, 0.5); */
  border-radius: 8px;
  transform: scale(0.8);
  opacity: 0.6;
}
.nth-left {
  transform-origin: right;
}
.nth-main {
  opacity: 1;
  transform: scale(1);
}
.nth-right {
  transform-origin: left;
}
/* .li:nth-child(1) {
  transform-origin: right;
}
.li:nth-child(3),
.li:nth-child(4) {
  transform-origin: left;
}
.li:nth-child(4) {
  margin-left: -48px;
}
.li:nth-child(2) {
  opacity: 1;
  transform: scale(1);
} */
.li:last-of-type {
  margin-right: 0;
}
</style>