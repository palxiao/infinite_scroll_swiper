<template>
  <div v-show="width" ref="swiper" :style="{ width }" class="swiper">
    <div
      class="ul"
      :style="{
        width: `${(+slidesPerView + 2) * (itemWidth + spaceBetween)}px`,
      }"
    >
      <my-img
        v-for="(item, i) in realImages"
        :key="i + 'i'"
        :style="{
          width: `${itemWidth}px`,
          marginRight: `${i > initialSlide || i < initialSlide - 1 ? 0 : spaceBetween}px`,
          marginLeft: i < initialSlide - 1 ? spaceBetween + 'px' : undefined,
        }"
        :class="['li', { 'nth-main': i === initialSlide }, { 'nth-left': i < initialSlide }, { 'nth-right': i > initialSlide }]"
        :src="item.image"
      />
    </div>
    <slot name="extra" />
  </div>
</template>

<script>
import myImg from './Image.vue'
export default {
  components: { myImg },
  props: {
    list: {
      default: () => [],
    },
    slidesPerView: {
      default: 3, // 一屏幕显示几个? 数字或者默认  auto 自动。
    },
  },
  data() {
    return {
      width: 0,
      spaceBetween: 48, // 图片之间的间距
      itemWidth: 0,
      initialSlide: 0, // 从第几个开始
      curIndex: 0,
      transTime: 500, // 过渡时间
      images: [],
      swiperOption: {
        autoplay: {
          delay: 3000,
        },
      },
    }
  },
  computed: {
    realImages() {
      return this.images.slice(0, Number(this.slidesPerView) + 2)
    },
  },
  watch: {
    list() {
      this.images = this.list
    },
    slidesPerView() {
      this.initialSlide = Math.floor(this.slidesPerView / 2)
      this.render()
    },
  },
  created() {
    this.initialSlide = Math.floor(this.slidesPerView / 2)
  },
  mounted() {
    this.images = this.list
    this.curIndex = this.initialSlide
    this.count = 0
    this.render()
  },
  methods: {
    async render() {
      await this.$nextTick()
      const dom = this.$refs.swiper
      const father = dom.parentElement || dom.parentNode
      this.width = father.offsetWidth + 'px'
      this.itemWidth = (father.offsetWidth - this.spaceBetween * (this.slidesPerView - 1)) / this.slidesPerView
    },
    domOperation(transition, left) {
      this._ulDom = this._ulDom || document.getElementsByClassName('ul')[0]
      this._ulDom.style.transition = transition
      this._ulDom.style.left = left
    },
    prev() {
      if (this._running) {
        return
      }
      this._running = true
      this.count = this.curIndex
      this.curIndex = this.count - 1 < 0 ? this.images.length - 1 : this.count - 1
      this.images.unshift(this.images[this.images.length - 1])
      this.domOperation('0s', `-${this.itemWidth + this.spaceBetween}px`)
      setTimeout(() => {
        this.domOperation(`${this.transTime / 1000}s`, '0px')
        setTimeout(() => {
          this.images.pop()
          this.done()
        }, this.transTime)
      }, 10)
    },
    next() {
      if (this._running) {
        return
      }
      this._running = true
      clearTimeout(this.timer1)
      this.count++
      this.initialSlide++
      this.domOperation(`${this.transTime / 1000}s`, `-${this.itemWidth * this.count + this.spaceBetween}px`)
      this.timer1 = setTimeout(() => {
        if (this.count) {
          let newIndex = (this.curIndex + this.count) % this.images.length
          newIndex === this.images.length && (newIndex = 0)
          this.curIndex = newIndex

          const step = this.count % this.images.length
          for (let i = 0; i < step; i++) {
            this.images.push(this.images.shift())
          }

          this.initialSlide--
          this.domOperation('0s', '0px')
          this.done()
        }
      }, this.transTime)
    },
    done() {
      this.count = 0
      this._running = false
      this.$emit('change', this.curIndex)
    },
  },
}
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
  /* display: flex; */
  height: 100%;
  left: 0;
  padding: 0;
  margin: 0;
  text-align: left;
}
.li {
  height: 100%;
  display: inline-block;
  /* display: flex;
  align-items: center;
  justify-content: center; */
  font-size: 30px;
  font-weight: 800;
  background: #001b57;
  /* box-shadow: 0px 6px 20px 0px rgba(0, 79, 255, 0.5); */
  border-radius: 8px;
  transform: scale(0.8);
  opacity: 0.6;
  object-fit: cover;
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