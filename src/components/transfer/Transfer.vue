<!-- 自定义公共组件: transfer
    author -- liulinfen
    @leftData 左侧数据
    @rightData 右侧数据
    @titles 数组数据,可以不设置，左右穿梭框的标题
    @sortType 上下排序的类型，可选值为： left、right、both三选一，可以不选，默认右侧可进行上下排序
 -->
<template>
  <div class="packaged_transfer">
    <div class="left_panel">
      <div class="header">
        <div class="title" v-if="titles && titles.length">{{titles[0]}}</div>
        <div @click="cancel('left')" class="cancelChoose">取消</div>
      </div>
      <el-scrollbar style="height: 339px;">
        <div class="content-box">
          <el-radio-group v-model="left" class="packaged_transfer_checkbox">
            <el-radio v-for="(item, index) in leftData" :label="index" :key="item.id">{{item.name}}</el-radio>
          </el-radio-group>
        </div>
      </el-scrollbar>
    </div>
    <div class="option-container">
      <div class="option_item" v-if="left || left === 0"  @click="transferData('left')"><i class="el-icon-arrow-right"></i></div>
      <div class="option_item disabled" v-else><i class="el-icon-arrow-right"></i></div>
      <div class="option_item" v-if="right || right === 0" @click="transferData('right')"><i class="el-icon-arrow-left"></i></div>
      <div class="option_item disabled" v-else><i class="el-icon-arrow-left"></i></div>
      <div class="option_item" v-if="upActive" @click="handleSort('up')"><i class="el-icon-arrow-up"></i></div>
      <div class="option_item disabled" v-else><i class="el-icon-arrow-up"></i></div>
      <div class="option_item" v-if="downActive" @click="handleSort('down')">
        <i class="el-icon-arrow-down"></i>
      </div>
      <div class="option_item disabled" v-else><i class="el-icon-arrow-down"></i></div>
    </div>
    <div class="right_panel">
      <div class="header">
        <div class="title" v-if="titles && titles.length && titles.length == 2">{{titles[1]}}</div>
        <div @click="cancel('right')" class="cancelChoose">取消</div>
      </div>
      <el-scrollbar style="height: 339px;">
        <div class="content-box">
          <el-radio-group v-model="right" class="packaged_transfer_checkbox">
            <el-radio v-for="(item, index) in rightData" :label="index" :key="item.id">{{item.name}}</el-radio>
          </el-radio-group>
        </div>
      </el-scrollbar>
    </div>
  </div>  
</template>
<script>
export default {
  data() {
    return {
      left: '',
      right: ''
    }
  },
  computed: {
    upActive() {
      return !this.sortType || this.sortType == 'right' ? this.right : (this.sortType == 'left' ? this.left : this.left || this.right);
    },
    downActive() {
      let con = false;
      if (!this.sortType || this.sortType == 'right') {
        con = (this.right || this.right === 0) && (this.right !== this.rightData.length - 1)
      } else if (this.sortType == 'left') {
        con = (this.left || this.left === 0) && (this.left !== this.leftData.length - 1)
      } else {
        con = (this.right || this.right === 0) && (this.right !== this.rightData.length - 1) || (this.left || this.left === 0) && (this.left !== this.leftData.length - 1)
      }
      return con;
    }
  },
  props: ['leftData', 'rightData', 'titles', 'sortType'],
  methods: {
    // 穿梭的回调
    transferData(type) {
      this.$emit('transfer', type, this[type], this.callback)
    },
    // 选中的索引置空
    callback(type) {
      this.cancel(type)
    },
    cancel(type) {
      if (type == 'both') {
        this.left = '';
        this.right = '';
      } else {
        this[type] = '';
      }
    },
    // 排序的回调
    handleSort(type) {
      let data = [this.left, this.right];
      this.$emit('handleSort', type, this.sortType, data, this.handleNewSort);
    },
    handleNewSort(sortType, array) {
      if (!sortType || sortType == 'right') {
        this.right = array[1]
      } else if (sortType == 'left') {
        this.left = array[0];
      } else if (sortType == 'both') {
        this.left = array[0];
        this.right = array[1];
      }
    }
  }
}
</script>
<style lang="scss" scoped>
  @import './index';
</style>
