<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮，sub方法在下面自定义了 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{ num }}</span>
    <!-- 加 1 的按钮， add方法在下面自定义了-->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
import bus from '@/components/eventBus.js'

export default {
  props: {
    // 接收商品的 id 值，将来，使用 EventBus 方案，
    // 把数量传递到 App.vue 的时候，需要通知 App 组件，更新哪个商品的数量
    id: {
      required: true,
      type: Number 
    },
    
    // 接收从 Goods 组件（父） 传递来的 num 数量值
    num: {
      type: Number,
      default: 1
    }
  },
  methods: {
    // 点击按钮，数值 +1
    add() {
      // 要发送给 App 的数据格式为 { id, value }
      // 其中，id 是商品的 id; value 是商品最新的购买数量
      // 自定义一个对象obj， 把对象保存到obj 中
      // 注意，add 中 value ： this.num + 1
      // obj 中 id 也 传了

      const obj = { id: this.id, value: this.num + 1 }
      // 要做的事情：通过 EventBus 把 obj 对象，发送给 App.vue 组件， Counter 组件 和 App 组件， 因为 不属于 直接 的 父子组件， 所以  属于 兄弟组价： -- 兄弟组价 之间 的 传值， 要用到 eventBus.js --: 先导入 eventBus.js, 发送方 用 bus.$emit(), 接收方 用 bus.$on()  
      bus.$emit('share', obj)
    },
    sub() {
      // 需要注意的是， 加一个判断 条件， 如果 num 已经 减到 1 了， 直接 退出：-- return 。
      if (this.num - 1 === 0) return
      // 要发送给 App 的数据格式为 { id, value }
      // 其中，id 是商品的 id; value 是商品最新的购买数量
      // 自定义一个对象obj， 把对象保存到obj 中
      // 注意，sub 中 value ： this.num - 1
      // obj 中 id 也 传了

      const obj = { id: this.id, value: this.num - 1 }
      // 要做的事情：通过 EventBus 把 obj 对象，发送给 App.vue 组件
      bus.$emit('share', obj)
    }
  },
  components: {
    bus
  }
}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
