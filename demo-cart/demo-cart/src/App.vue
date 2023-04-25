<template>
  <div class="app-container">
    <!-- Header 头部 区域  -->
    <Header></Header>

    <!-- fullState 在 使用 的 时候， 还是 当做 一个 普通 的 属性 使用 -->
    <!-- <p>{{ fullState }} </p> -->

    <!-- <p>{{ sumPrice }} </p> -->

    <!-- 循环 渲染 每一个 商品 的 信息 -->
    <!-- item in list 遍历 数组 的 每一项， 注意v-for 要绑定key , 每一项 的 id 作为  key: :key="item.id" -->
    <!-- @state-change="getNewState"中， state-change 是 子组件（Goods.vue） 中 this.$emit 中 的 第一个 参数， 自定义的事件类型， getNewState是 在自己中 methods 中 定义的 方法。getNewState(e) 中的 e 就是 子组件中 this.$emit 方法中的 第二个参数， 传递的数据。   -->
    <Goods v-for="item in list" :key="item.id" :id="item.id" :title="item.goods_name" :pic="item.goods_img" :count="item.goods_count" :price="item.goods_price" :state="item.goods_state" @state-change="getNewState"  ></Goods>


   


    <!-- Footer 尾部区域 -->
    <!-- 父向子传值 ， isfull 是 Footer 中 自定义的 属性 。 把计算属性的 值 赋值 给 isfull。 -->
    <!-- 子向父传值，父组件中定义的@事件， 是在使用子组件的标签名中定义的！！！ -->
    <Footer :isfull="fullState" :totalprice="sumPrice" :all="totalnum" @full-change="noChoose" ></Footer>
  </div> 
</template>


<script>
// 导入 axios请求库， 用axios 接收， 库的名字也叫axios
import axios from 'axios' 

//  导入 Header 组件  
import Header from '@/components/Header/Header.vue'
// 导入 Goods 组件
import Goods from '@/components/Goods/Goods.vue'
// 导入 Footer 组件
import Footer from '@/components/Footer/Footer.vue'
// 导入 eventBus.js 
import bus from '@/components/eventBus.js'

  export default {
    components: {
      Header,
      Goods,
      Footer,
      bus 
    },
    data() {
      return {
        // 用来存储返回的 数据的列表数组（res.list） ， 初始化为空
        list: []
      }
    },

    // 定义一个计算属性, 动态计算出  一个 布尔值 属性 ， 根据 每一项 商品 的 checked ， 动态 得到 一个 布尔值， 然后 这个 布尔值 赋值 给 Footer组件 中 的 checked 使用。 
    computed: {
      // 动态计算出 全选 的 状态 是 true 还是 false， 定义一个 方法， fullState()
      // 计算属性 在定义的时候 必须 定义成方法， 使用的时候 当成 普通属性 使用
      fullState() {
        return  this.list.every( item => {
          return item.goods_state == true //一定要记得 every 的 判断条件 前 要加 return 
        } )
      },
      sumPrice() {
        // 需要注意的是，先过滤得到 checked 为 true的 商品，用filter方法， 记得回调函数中的 判断条件 要加 return 
        return this.list.filter( item => {
          return item.goods_state == true
        } ).reduce( (num, item) => {
          return num += item.goods_count*item.goods_price
        }, 0 )
      },
      // 已勾选商品的总数量, 用来 放在 Footer 的 结算 中
      totalnum() {
        // 先要过滤，得到所有勾选的 商品
        return this.list.filter( item => {
          return item.goods_state == true 
        }).reduce( (num,item) => {
          return num += item.goods_count
        }, 0)
      }
    },

    methods: {
    
      // 请求数据的方法: initCartList 方法定义在 methods 中， 在 created 生命周期函数 中 调用
       async initCartList() {
        // 使用axios 发送请求, 前面加await （因为 axios 返回的是 一个 Promise 对象。）
        // 解构赋值， 因为 axios 返回的对象 ， 外面会加工， 里面的 data 属性， 才是 真正服务器的数据，所以-- const {data}, 并且 重命名 为 res （data:res）
            const {data: res} = await axios.get('https://www.escook.cn/api/cart')
            // console.log(res)
            // 当请求成功时，--: (res.status == 200) 就把res.list 赋值给 this.list
            if (res.status == 200) {
              this.list = res.list
            }
            console.log(this.list);
       },

       getNewState(e) {
         let id = e.id
         let newState = e.state 
         this.list[id-1].goods_state = newState

        //  法二： 用some方法： 
        // this.list.some( item => {

        //   if (item.id === e.id) {
        //     item.goods_state = e.state
        //     return true // 终止后续循环
        //   }
        // } )


       },

       // 接收 Footer 组件 传递过来 的 checked 的 值
       noChoose(e) {
        // 让 list 中 的 每一项 的 goods_state 都 为 传过来的 e 的值， e 为 Footer 中 checked 的 值 
          this.list.forEach( item => item.goods_state = e)
       }

    },
    created() {
      this.initCartList()
      bus.$on('share', (obj) => {
        this.list.some( item => {
          if (item.id == obj.id) {
           item.goods_count = obj.value 
           return true// 注意， 一定要加 return ，return true 表示 提前 退出 循环。 
          }
        })
      })
    }
  }
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
  h1 {
    font-size: 2rem;
  }
}
</style>
