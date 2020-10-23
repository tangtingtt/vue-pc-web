<template>
  <div class="templete-list-wrapper">
    <!-- 资讯列表 -->
    <div class="templete-list-article-list" v-if="id==1">
      <ul>
        <li v-for="item in 5" :key="item"></li>
      </ul>
    </div>
    <!-- 走马灯 -->
    <div class="templete-list-1" v-if="id==2">
      <el-carousel height="60px">
        <el-carousel-item v-for="item in 4" :key="item">
          <h3 class="small">{{ item }}</h3>
        </el-carousel-item>
      </el-carousel>
    </div>
    <div class="templete-list-1" v-if="id==3">
      模板三
    </div>
  </div>
</template>
<script>
export default {
  name: 'TempleteList',
  components: {},
  directives: {},
  filters: {},
  extends: {},
  mixins: [],
  props: {
    id: {
      type: String,
      default: '0'
    }
  },
  data () {
    return {
    }
  },
  computed: {},
  watch: {},
  beforeCreate () {},
  created () {},
  beforeMount () {},
  mounted () {},
  beforeUpdate () {},
  updated () {},
  activated () {},
  deactivated () {},
  beforeDestroy () {},
  destroyed () {},
  methods: {}
}
</script>
<style lang="scss" scoped>
.templete-list-wrapper {
  text-align: left;
}
.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
  }
.el-carousel__item:nth-child(2n+1) {
  background-color: #d3dce6;
}
.templete-list-article-list {
  font-size: 16px;
  font-weight: 400;
  color: #3d4552;
  ul {
    margin: 0;
    padding: 0;
  }
  li {
    cursor: pointer;
    margin: 8px 0;
    background: #e5e9f2;
    list-style: none;
    line-height: 24px;
    height: 24px;
    border-radius: 4px;
    &:first-child {
      margin-top: 0px;
    }
  }
}
</style>
