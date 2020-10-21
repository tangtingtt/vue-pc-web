<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
      <grid-layout
        :layout="layoutData"
        :col-num="12"
        :row-height="layoutHeight"
        :is-draggable="dialogVisible"
        :is-resizable="dialogVisible"
        :is-mirrored="false"
        :vertical-compact="true"
        :margin="[10, 10]"
        :use-css-transforms="true"
       >
         <grid-item v-for="(item,index) in layoutData"
                    :x="item.x"
                    :y="item.y"
                    :w="item.w"
                    :h="item.h"
                    :i="item.i"
                    :key="item.i"
         >
           {{index}}
         </grid-item>
       </grid-layout>
  </div>
</template>

<script>
// @ is an alias to /src
import layoutData from '@/assets/json/layoutData.json'
// import HelloWorld from '@/components/HelloWorld.vue'
import VueGridLayout from 'vue-grid-layout'

const GridLayout = VueGridLayout.GridLayout
const GridItem = VueGridLayout.GridItem

export default {
  name: 'Home',
  data () {
    return {
      // 布局数据
      layoutData: [],
      layoutConfig: {
        height: 100, // 默认高度
        dialogVisible: false // 是否可拖拽或改变大小
      }
    }
  },
  components: {
    // HelloWorld,
    GridLayout,
    GridItem
  },
  methods: {
    init () {
      this.layoutData = layoutData.mainData
    }
  },
  created () {
    this.init()
  }
}
</script>
<style scoped>
.vue-grid-item {
  background: aquamarine;
}
</style>
