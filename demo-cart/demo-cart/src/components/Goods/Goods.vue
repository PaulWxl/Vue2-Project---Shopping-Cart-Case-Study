<template>
  <div class="goods-container">
    <!-- 左侧图片 -->
    <div class="thumb">
      <div class="custom-control custom-checkbox">
        <!-- 复选框 -->
        <!-- 这里的 checked 不用v-model 是因为 state 在 props 中， props 是只读的  , 处理方法： 子向父传值， 自定义方法 -->
        <!-- 复选框本身就有一个 change 事件。 @change="" -->
        <input type="checkbox" class="custom-control-input" :id="'cb' + id" :checked="state" @change="stateChange" />
        <label class="custom-control-label" :for="'cb' + id">
          <!-- 商品的缩略图 -->
          <img :src="pic" alt="" />
        </label>
      </div>
    </div>
    <!-- 右侧信息区域 -->
    <div class="goods-info">
      <!-- 商品标题 -->
      <h6 class="goods-title">{{ title }}</h6>
      <div class="goods-info-bottom">
        <!-- 商品价格 -->
        <span class="goods-price">￥{{ price }}</span>
        <!-- 商品的数量 -->
        <!-- :num 是 向 Counter 组件 中 传递 商品 数量  -->
        <!-- :id 是 向 Counter 组件 中 传递 商品 的 id  -->
        <Counter :num="count" :id="id"></Counter>
      </div>
    </div>
  </div>
</template>

<script>
// Goods 组件 直接 用到了 Counter 组件， 所以 要导入。 
// import Counter from '@/components/Counter/Counter.vue'

export default {
  props: {
    // 商品的 id
    // 为啥在这里要封装一个 id 属性呢？
    // 原因:将来,子组件中商品的勾选状态变化之后, 需要通过子 -> 父的形式,

    // 通知父组件根据 id 修改 对应 商品的勾选状态。 父组件 得先拿到商品的id 才知道对应更改哪一件商品的 state 
    id: {
      // requeired 为 true ，表示必填项, 对于 id  来说， default 没必要。 
      required: true,
      type: Number
    },

    // 为了 父向子传值， 传入 title ， 所以 自定义属性
    title: {
      default: '',
      type: String 
    },

    // 要渲染的商品的图片路径 作为src绑定： :src="pic"
    pic: {
      default: '',
      type: String
    },

    // 商品的单价
    price: {
      default: 0,
      type: Number
    },

    // 商品的购买数量
    count: {
      default: 1,// 初始化为1
      type: Number,
    },

    // 商品的勾选状态
    state: {
      default: true,
      type: Boolean
    },

    
  },

  methods: {
    // 只要复选框的选中状态发生了变化，就会调用这个处理函数
    // stateChange(e) {
    //   const newState = e.target.checked
    //   // 触发自定义事件
    //   this.$emit('state-change', { id: this.id, value: newState })
    // }
    stateChange(e) {
      // 使用事件的默认形参e 很重要， e.target.checked 可以 拿到 勾选状态的 方法
      let newState = e.target.checked
      this.$emit('state-change', {
        id: this.id,
        state: newState// newState 赋值给 state, 然后 作为 参数 传递 给 
      })
    }
  },
  components: {
    // Counter
  }
}
</script>

<style lang="less" scoped>
.goods-container {
  + .goods-container {
    border-top: 1px solid #efefef;
  }
  padding: 10px;
  display: flex;
  .thumb {
    display: flex;
    align-items: center;
    img {
      width: 100px;
      height: 100px;
      margin: 0 10px;
    }
  }

  .goods-info {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    flex: 1;
    .goods-title {
      font-weight: bold;
      font-size: 12px;
    }
    .goods-info-bottom {
      display: flex;
      justify-content: space-between;
      .goods-price {
        font-weight: bold;
        color: red;
        font-size: 13px;
      }
    }
  }
}
</style>
