<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <x-swiper :list="list" @itemClick="swiperClick">
      <div slot="desc" style="color:#fff;">{{name}}</div>
    </x-swiper>
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import xSwiper from '../packages'
import img from '@/assets/cecilia-g.jpg'
import img2 from '@/assets/cecilia.jpg'
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    xSwiper,
    HelloWorld
  },
  data() {
    return {
      list: [
        { img, img2, name: '张三1' },
        { img, img2, name: '张三2' },
        { img, img2, name: '张三3' },
        { img, img2, name: '张三4' },
        { img, img2, name: '张三5' },
        { img, img2, name: '张三6' },
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

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
