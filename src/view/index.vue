<template>
  <div>
    <transfer :leftData="leftData" :rightData="rightData" :titles="['左边数据', '右边数据']" @transfer="handleTransfer" @handleSort="handleSort">
    </transfer>
  </div>
</template>
<script>
import Transfer from '@/components/transfer/Transfer';
export default {
  name: 'index',
  data() {
    return {
      leftData: [
        { name: '北京', id: 1 }, { name: '上海', id: 2 }, { name: '南京', id: 3 }, { name: '天津', id: 4 }, { name: '广东', id: 5 }
      ],
      rightData: [{ name: '杭州', id: 6 }, { name: '越南', id: 7 }, { name: '厦门', id: 8 }]
    }
  },
  components: {
    Transfer
  },
  methods: {
    handleTransfer(type, data, callback) {
      if (type == 'left') {
        this.rightData = this.rightData.concat(this.leftData[data]);
      } else {
        this.leftData = this.leftData.concat(this.rightData[data])
      }
      this[type + 'Data'].splice(data, 1);
      callback(type);
    },
    handleSort(type, sortType, data, callback) {
      if (!sortType || sortType == 'right') {
        if (type == 'up') {
          this.rightData = this.swapItems(this.rightData, data[1], data[1] - 1)
          callback(sortType, [data[0], data[1] - 1]);
        } else {
          this.rightData = this.swapItems(this.rightData, data[1], data[1] + 1)
          callback(sortType, [data[0], data[1] + 1])
        }
      } else if (sortType == 'left') {
        if (type == 'up') {
          this.leftData = this.swapItems(this.leftData, data[0], data[0] - 1)
          callback(sortType, [data[0] - 1, data[1]]);
        } else {
          this.leftData = this.swapItems(this.leftData, data[0], data[0] + 1)
          callback(sortType, [data[0] + 1, data[1]])
        }
      } else if (sortType == 'both') {
        if (type == 'up') {
          data[0] == 0 ? '' : this.leftData = this.swapItems(this.leftData, data[0], data[0] - 1)
          data[1] == 0 ? '' : this.rightData = this.swapItems(this.rightData, data[1], data[1] - 1)
          callback(sortType, [data[0] == 0 ? data[0] : (data[0] - 1), data[1] == 0 ?  data[1] : (data[1] - 1)]);
        } else {
          data[0] == (this.leftData.length - 1) ? '' : this.leftData = this.swapItems(this.leftData, data[0], data[0] + 1)
          data[1] == (this.rightData.length - 1) ? '' : this.rightData = this.swapItems(this.rightData, data[1], data[1] + 1)
          callback(sortType, [data[0] == (this.leftData.length - 1) ? data[0] : (data[0] + 1), data[1] == (this.rightData.length - 1) ? data[1] : (data[1] + 1)])
        }
      }
    },
    swapItems(arr, index1, index2) {
      arr[index1] = arr.splice(index2, 1, arr[index1])[0];
      return arr;
    }
  }
}
</script>

