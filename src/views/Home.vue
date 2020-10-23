<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div class="layout-opt">
      <div class="layout-opt-btn" @click="addItem"><i class="el-icon-plus"></i>添加组件</div>
      <el-button type="primary" @click="editPage">编辑</el-button>
      <el-button type="primary" @click="previewPage">预览</el-button>
    </div>
    <div class="layout-box">
      <grid-layout
        :layout="layoutData"
        :col-num="12"
        :row-height="layoutConfig.layoutHeight"
        :is-draggable="layoutConfig.dialogVisible"
        :is-resizable="layoutConfig.resizableVisible"
        :is-mirrored="false"
        :vertical-compact="true"
        :margin="layoutConfig.marginArr"
        :use-css-transforms="true"
        @layout-created="layoutCreatedEvent"
        @layout-before-mount="layoutBeforeMountEvent"
        @layout-mounted="layoutMountedEvent"
        @layout-ready="layoutReadyEvent"
        @layout-updated="layoutUpdatedEvent"
       >
         <grid-item v-for="(item,index) in layoutData"
                    :x="item.x"
                    :y="item.y"
                    :w="item.w"
                    :h="item.h"
                    :i="item.i"
                    :key="index"
                    @resize="resizeEvent"
                    @move="moveEvent"
                    @resized="resizedEvent"
                    @moved="movedEvent"
                    :class="layoutConfig.dialogVisible?'vue-grid-item-edit':''"
         >
           <!-- {{item.i}} -->
           <p v-if="layoutConfig.dialogVisible" class="grid-item-opt">
            <span @click="editComponent(item.i)" class="edit-component"><i class="el-icon-edit"></i></span>
            <span @click="deleteComponent(item.i)" class="delete-component"><i class="el-icon-delete"></i></span>
           </p>
           <div class="grid-item-content">
              <TempleteList
                :id="item.component"
              >
              </TempleteList>
           </div>
         </grid-item>
       </grid-layout>
    </div>
    <el-dialog
      :title="dialogTitle"
      :visible.sync="dialogVisible"
      width="50%"
      @close="handleClose">
      <div>
        <div class="chooseTemplete">
          <div class="templeteArr-title">选择模板</div>
          <div class="templeteArr-item"
            v-for="(item,index) in templeteArr"
            :key="index"
            @click="chooseTemplete(item)"
            :class="item.id==currentTemplete?'templeteArr-item-active':''"
          >
          {{item.name}}
          </div>
        </div>
        <div class="templetePreview">
          <div class="templeteArr-title">模板预览</div>
          <div class="templete-preview">
            <TempleteList
              :id="currentTemplete"
            >
            </TempleteList>
          </div>
        </div>
      </div>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
// @ is an alias to /src
import layoutData from '@/assets/json/layoutData.json'
import TempleteList from '@/components/templeteList.vue'
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
        marginArr: [30, 30], // 网格之间的边距 两个数字组成的数组 第一个数字为水品距离 第二个为垂直距离 非必填 默认值为 [10,10]
        height: 100, // 默认高度
        dialogVisible: true, // 是否可拖拽
        resizableVisible: true // 是否可改变大小
      },
      dialogVisible: false,
      dialogTitle: '',
      templeteArr: [
        { id: '1', name: '模板一' },
        { id: '2', name: '模板二' },
        { id: '3', name: '模板三' }
      ],
      currentTemplete: '1', // 当前模板
      currentComponent: null, // 当前组件
      componentTempleteIds: {} // 组件模板绑定
    }
  },
  components: {
    TempleteList,
    GridLayout,
    GridItem
  },
  methods: {
    handleClose () {
      this.$set(this.componentTempleteIds, this.currentComponent, this.currentTemplete)
      this.layoutData.forEach((item) => {
        if (item.i === this.currentComponent) {
          this.$set(item, 'component', this.currentTemplete)
        }
      })
      console.log('----编辑节点---', this.currentTemplete, this.componentTempleteIds, '00000', this.layoutData)
    },
    // 选择模板
    chooseTemplete (item) {
      this.currentTemplete = item.id
    },
    init () {
      this.layoutData = layoutData.mainData
    },
    // 预览页面
    previewPage () {
      this.layoutConfig.dialogVisible = false
      this.layoutConfig.resizableVisible = false
    },
    // 编辑页面
    editPage () {
      this.layoutConfig.dialogVisible = true
      this.layoutConfig.resizableVisible = true
    },
    // 布局创建事件
    layoutCreatedEvent (newLayout) {
      console.log('----布局创建事件---', newLayout)
    },
    // 布局安装以前事件
    layoutBeforeMountEvent (newLayout) {
      console.log('----布局安装以前事件---', newLayout)
    },
    // 布局安装事件
    layoutMountedEvent (newLayout) {
      console.log('----布局安装事件---', newLayout)
    },
    // 布局准备活动
    layoutReadyEvent (newLayout) {
      console.log('----布局准备活动---', newLayout)
    },
    // 格子位置 大小更新
    layoutUpdatedEvent (newLayout) {
      console.log('----格子位置 大小更新---', newLayout)
    },
    resizeEvent (i, newH, newW, newHPx, newWPx) {
      console.log('调整大小时的事件RESIZE i=' + i + ', H=' + newH + ', W=' + newW + ', H(px)=' + newHPx + ', W(px)=' + newWPx)
    },
    moveEvent (i, newX, newY) {
      console.log('移动时的事件MOVE i=' + i + ', X=' + newX + ', Y=' + newY)
    },
    resizedEvent (i, newH, newW, newHPx, newWPx) {
      console.log('调整大小后的事件RESIZED i=' + i + ', H=' + newH + ', W=' + newW + ', H(px)=' + newHPx + ', W(px)=' + newWPx)
    },
    movedEvent (i, newX, newY) {
      console.log('移动后的事件MOVED i=' + i + ', X=' + newX + ', Y=' + newY)
    },
    // 获取当前数组最后一行是第几行
    getLayoutDataTwoStr () {
      const tempx = []
      const tempy = []
      const tempi = []
      this.layoutData.map((item) => {
        tempx.push(item.x)
        tempy.push(item.y)
        tempi.push(item.i)
      })
      return { tempx: tempx, tempy: tempy }
    },
    // 添加节点
    addItem () {
      const len = this.layoutData.length
      console.log(this.getLayoutDataTwoStr(), '---', Math.max(...this.getLayoutDataTwoStr().tempy) + 1)
      this.layoutData.push({
        x: 0,
        y: Math.max(...this.getLayoutDataTwoStr().tempy) + 1,
        w: 12,
        h: 1,
        i: len
      })
    },
    // 删除节点
    deleteComponent (i) {
      console.log('------el----', i)
      this.layoutData = this.layoutData.filter(
        item => item.i !== i
      )
      // 重新排列i
      // this.layoutData.forEach((item, index) => {
      //   item.i = index
      // })
      // console.log('---重绘i----', this.layoutData)
    },
    // 编辑节点
    editComponent (i) {
      this.dialogTitle = '编辑组件' + i
      this.currentComponent = i
      this.dialogVisible = true
    }
  },
  created () {
    this.init()
  }
}
</script>
<style scoped>
.vue-grid-item {
  border-radius: 5px;
}
.vue-grid-item-edit {
  border: 1px dashed #999;
}
.layout-box {
  width: 100%;
  border: 1px solid #DFE1E5;
  min-height: 1200px;
}
.home {
  width: 1200px;
  margin: 60px auto;
}
.layout-opt {
  margin: 25px 0;
  text-align: left;
}
.layout-opt-btn {
  color: #344563;
  font-size: 14px;
  display: inline-block;
  font-weight: bold;
  padding: 6px 12px;
  cursor: pointer;
}
.layout-opt-btn:hover {
    background-color: #e6e5e5;
    border-radius: 3px;
}
.delete-component{
  cursor: pointer;
  display: inline-block;
  height: 40px;
  width: 40px;
  line-height: 40px;
  text-align: center;
}
.edit-component{
  cursor: pointer;
  display: inline-block;
  height: 40px;
  width: 40px;
  line-height: 40px;
  text-align: center;
}
.chooseTemplete {
  width: 200px;
  font-size: 16px;
  display: inline-block;
}
.templeteArr-title {
  line-height: 32px;
  color: #b1afaf;
}
.templeteArr-item {
  padding: 10px 0;
  cursor: pointer;
}
.templeteArr-item-active {
  background-color: #efebeb;
  font-weight: bold;
}
.templetePreview {
  width: calc(100% - 300px);
  display: inline-block;
  vertical-align: top;
}
.templete-preview {
  margin: 30px;
}
.grid-item-opt {
  position: absolute;
  right: 0;
  margin: 0;
}
</style>
