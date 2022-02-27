<template>
  <div class="swiper-item" :style="styles" :class="active && 'active'">
      <img :src="item.img" alt="" class="img">
      <img :src="item.img2" alt="" class="img2">
      <div class="name">{{item.name}}</div>
  </div>
</template>

<script>
export default {
  props: {
    item: { // 轮播图列表
      type: Object,
      default: () => {
        return {}
      }
    },
    curr: {
      type: Boolean,
      default: false
    },
    active: {
      type: Boolean,
      default: false
    },
    type: {
      type: Array,
      default: () => {
        return ['left', 'in']
      }
    },
    time: {
      type: Number,
      default: 100
    }
  },
  data() {
    return {
      styles: {}
    };
  },
  watch: {
    curr(val) {
      if (val) {
        this.init()
      }
    }
  },
  methods: {
    init() {
      let s = {}
      let d = this.type[0]
      let t = this.type[1]
      let time = this.time / 20
      if (d == 'left') { // 往左
        if (t == 'in') { // 进
          s = {left: '100%', opacity: 0}
        } else { // 出
          s = {left: 0, opacity: 1}
        }
      } else { // 往右
        if (t == 'in') { // 进
          s = {left: '-100%', opacity: 0}
        } else { // 出
          s = {left: 0, opacity: 1}
        }
      }
      this.styles = s
      let p = 100
      let timer = setInterval(() => {
        p = p - 5
        if (d == 'left') { // 往左
          if (t == 'in') { // 进
            s = {left: p + '%', opacity: (100 - p) / 100}
          } else { // 出
            s = {left: (p - 100) + '%', opacity: p / 100}
          }
        } else { // 往右
          if (t == 'in') { // 进
            s = {left: -p + '%', opacity: (100 - p) / 100}
          } else { // 出
            s = {left: (100 - p) + '%', opacity: p / 100}
          }
        }
        this.styles = s
        if (p <= 0) {
          clearInterval(timer)
        }
      }, time)
    }
  }
}
</script>

<style scoped>
.swiper-item{position: relative;opacity: 0;}
.swiper-item img{width:100%;display: block;}
.swiper-item .img2{display:none;}
.swiper-item .name{font-size: 20px;line-height: 50px;text-align: center;background: #222;color:#b1935e;}
.swiper-item:hover .img, .swiper-item.active .img{display: none;}
.swiper-item:hover .img2, .swiper-item.active .img2{display: block;}
.swiper-item:hover .name, .swiper-item.active .name{background: #b1935e;color:#222;}
</style>
