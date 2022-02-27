<template>
  <div class="swiper" :style="{width: swiperWidth + 'px'}">
    <div class="swiper-box">
      <div class="item" @click="itemClick(index, item)" v-for="(item, index) in showList" :key="index" :style="{maxWidth: (swiperWidth / num) + 'px'}">
        <x-swiper-item :item="item" :curr="currItem - 1 == index" :active="activeItem == index" :type="type" :time="aTime"></x-swiper-item>
      </div>
    </div>
    <div v-if="this.activeItem >= 0">
      <slot name="desc"></slot>
    </div>
    <div class="swiper-handle" v-if="list.length / num > 1">
      <div class="left" @click="clickLeft">&lt;</div>
      <div class="right" @click="clickRight">&gt;</div>
      <div class="dot-box">
        <div class="dot" @click="dotClick(i)" :class="dotIndex + 1 == i ? 'active' : ''" v-for="i in dotNum" :key="i"></div>
      </div>
    </div>
  </div>
</template>

<script>
import xSwiperItem from './x-swiper-item'
export default {
  components: { xSwiperItem },
  props: {
    swiperWidth: { // 轮播图宽度，px
      type: Number,
      default: 1000
    },
    num: { // 展示图片数量 
      type: Number,
      default: 4
    },
    list: { // 轮播图列表
      type: Array,
      default: () => {
        return []
      }
    }
  },
  data() {
    return {
      dotIndex: 0,
      showList: this.list.slice(0, 4),
      currItem: 0,
      activeItem: -1,
      type: ['left', 'in'],
      aTime: 200,
      disabled: false
    };
  },
  computed: {
    dotNum() {
      return Math.ceil(this.list.length / this.num)
    }
  },
  created() {
    this.init()
  },
  methods: {
    dotClick(i) {
      if (this.dotIndex == i - 1) return
      if (this.dotIndex > i - 1) {
        this.clickLeft()
      } else {
        this.clickRight()
      }
    },
    itemClick(i, item) {
      this.activeItem = this.activeItem == i ? -1 : i
      this.$emit('itemClick', item)
    },
    init() {
      if (this.type[0] == 'left') {
        this.currItem == 1
      } else {
        this.currItem == this.showList.length
      }
      this.$nextTick(()=>{
        this.next()
      })
    },
    next() {
      let d = this.type[0]
      let t = this.type[1]
      setTimeout(() => {
        if (d == 'left') {
          if (this.currItem < this.showList.length + 1) {
            this.currItem++
            if (this.currItem == this.showList.length + 1 && t == 'out') {
              this.leftIn()
            } else {
              this.next()
            }
          } else {
            if (t == 'in') {
              this.disabled = false
            }
          }
        } else {
          if (this.currItem > 0) {
            this.currItem--
            if (this.currItem == 0 && t == 'out') {
              this.rightIn()
            } else {
              this.next()
            }
          } else {
            if (t == 'in') {
              this.disabled = false
            }
          }
        }
      }, this.aTime)
    },
    clickLeft() {
      if (this.disabled) return
      this.disabled = true
      this.activeItem = -1
      this.dotIndex--
      if (this.dotIndex < 0) {
        this.dotIndex = this.dotNum - 1
      }
      this.rightOut()
    },
    clickRight() {
      if (this.disabled) return
      this.disabled = true
      this.activeItem = -1
      this.dotIndex++
      if (this.dotIndex > this.dotNum - 1) {
        this.dotIndex = 0
      }
      this.leftOut()
    },
    leftOut() {
      this.currItem = 1
      this.type = ['left', 'out']
      this.next()
    },
    leftIn() {
      this.showList = this.list.slice(this.dotIndex * this.num, (this.dotIndex + 1) * this.num)
      this.$nextTick(()=>{
        this.currItem = 1
        this.type = ['left', 'in']
        this.next()
      })
    },
    rightOut() {
      let currList = this.showList
      this.currItem = currList.length
      this.type = ['right', 'out']
      this.next()
    },
    rightIn() {
      this.showList = this.list.slice(this.dotIndex * this.num, (this.dotIndex + 1) * this.num)
      this.$nextTick(()=>{
        this.currItem = this.showList.length
        this.type = ['right', 'in']
        this.next()
      })
    }
  }
}
</script>

<style scoped>
.swiper{margin: 0 auto;background: #000;}
.swiper-box{display: flex;background: #222;}
.swiper-box .item{flex:1;}
.swiper-handle{color: #b1935e;height: 50px;line-height: 50px;font-size: 36px;text-align: center;}
.swiper-handle .left{float:left;width:80px;transform: scale(1,1.5);}
.swiper-handle .right{float:right;width:80px;transform: scale(1,1.5);}
.swiper-handle .dot-box{margin: 0 80px;line-height: 8px;padding:15px 0;}
.swiper-handle .dot-box .dot{width:8px;height:8px;border-radius: 50%;display: inline-block;background:#b1935e;margin:0 5px;opacity: 0.4;}
.swiper-handle .dot-box .dot.active{opacity: 1;}
</style>
