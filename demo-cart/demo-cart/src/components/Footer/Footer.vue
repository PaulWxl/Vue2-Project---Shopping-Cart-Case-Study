<template>
  <div class="footer-container">
    <!-- 左侧的全选 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="cbFull" :checked="isfull" @change="fullChange" />
      <label class="custom-control-label" for="cbFull">全选</label>
    </div>


    <!-- 中间的合计 -->
    <div>
      <span>合计：</span>
      <!-- 代码"totalprice.toFixed(2)"是JavaScript中的一个方法，用于将数字格式化为固定的小数位数，具体是将总价格显示为两个小数位。 ".toFixed(2)"方法用于精确地将该值格式化为仅有两个小数位。请注意，".toFixed()"方法返回一个字符串，而不是一个数字。如果您需要使用格式化后的数字进行计算，则可能需要使用parseFloat()或Number()函数将其转换回数字。 下面是用 parseFloat 转换为 Number 型， ParseFloat( totalprice.toFixed(2) ) -->
      <span class="total-price">￥{{ parseFloat(totalprice.toFixed(2)) }}</span>
      
    </div>

    <!-- 结算按钮 -->
    <button type="button" class="btn btn-primary btn-settle">结算（{{ all }}）</button>
  </div>
</template>

<script>
export default {
  props: {
    // 全选的状态, 用于 父向子 传值 的 作用, isfull 
    isfull: {
      type: Boolean,
      default: false 
    },
    // 总价格
    totalprice: {
      default: 0,
      type: Number 
    },
    // 已勾选的商品的总数量
    all: {
      type: Number,
      default: 0
    }
  },
  methods: {
    // 监听到了全选的状态变化
    fullChange(e) {
      this.$emit('full-change', e.target.checked)

    },

  }
}
</script>

<style lang="less" scoped>
.footer-container {
  font-size: 12px;
  height: 50px;
  width: 100%;
  border-top: 1px solid #efefef;
  position: fixed;
  bottom: 0;
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

.custom-checkbox {
  display: flex;
  align-items: center;
}

#cbFull {
  margin-right: 5px;
}

.btn-settle {
  height: 80%;
  min-width: 110px;
  border-radius: 25px;
  font-size: 12px;
  margin-right: 1rem;
}

.total-price {
  font-weight: bold;
  font-size: 1rem;
  color: red;
}
</style>
