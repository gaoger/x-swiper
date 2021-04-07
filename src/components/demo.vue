<template>
  <x-swiper :list="list" @itemClick="swiperClick">
    <div slot="desc" style="color:#fff;">{{name}}</div>
  </x-swiper>
</template>

<script>
import xSwiper from './x-swiper'
import logo from '@/assets/cecilia-g.jpg'
import logo2 from '@/assets/cecilia.jpg'
export default {
  name: 'App',
  components: {
    xSwiper,
  },
  data () {
    return {
      list: [
        {
          img: logo,
          img2: logo2,
          name: '张三'
        },
        {
          img: logo,
          img2: logo2,
          name: '张三2'
        },
        {
          img: logo,
          img2: logo2,
          name: '张三3'
        },
        {
          img: logo,
          img2: logo2,
          name: '张三4'
        },
        {
          img: logo,
          img2: logo2,
          name: '张三5'
        }
      ],
      name: ''
    }
  },
  methods: {
    swiperClick(item) {
      this.name = item.name
    }
  }
}
</script>
