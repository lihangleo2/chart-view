<template>
  <div>
    <button class="buttonStyle" @click="test">{{ buttonText }}</button>
    <div class="fatherCss" ref="fatherCss">
      <canvas-item
        :lineCenterX="50"
        :value_50="50"
        :textFont="14"
        ref="mychild"
      ></canvas-item>
    </div>
  </div>
</template>


<script>
import ChartView from '@/components/ChartView.vue'
import axios from 'axios'

export default {
  data() {
    return {
      //开始时间
      start: 0,
      //满屏展示的波，所占的时间
      showTotalTime: 20,
      //整个数据源的波长时间
      dataTotalTime: 210,
      //波的总数据源list
      list: [],
      //以下用于处理按钮点击
      isScroll: false,
      buttonText: '点击开始',
      timer: null,
    }
  },

  //部件
  components: {
    'canvas-item': ChartView,
  },
  //静态
  props: {},
  //对象内部的属性监听，也叫深度监听
  watch: {},
  //属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
  computed: {},
  //方法表示一个具体的操作，主要书写业务逻辑；
  methods: {
    initData() {
      //如果开始时间不得大于总时间
      if (this.start >= this.dataTotalTime) {
        return
      }

      //如果当前展示波的末尾时间大于总时间，那么末尾时间就等于总时间
      var lastTime = this.start + this.showTotalTime
      if (lastTime >= this.dataTotalTime) {
        lastTime = this.dataTotalTime
      }

      var dataList = []

      //用开始时间和总时间算出数据源起始index,取整
      let start = Math.floor(
        (this.list.length * this.start) / this.dataTotalTime
      )

      //同理算出末尾index
      let end = Math.floor((this.list.length * lastTime) / this.dataTotalTime)

      //从总数据获取当前所展示的数据
      dataList = Object.assign([], dataList, this.list.slice(start, end))
      this.$refs.mychild.initDataSon(dataList)

      //起始时间每次+20ms,因为下面开启了间隔20ms运行一次，模拟波的运动
      this.start += 0.02
    },
    test() {
      if (this.isScroll) {
        //代表是滚动时候
        this.isScroll = false
        this.buttonText = '点击开始'
        clearInterval(this.timer)
      } else {
        //未滚动时候
        this.isScroll = true
        this.buttonText = '点击暂停'
        this.initData()
        this.timer = setInterval(() => {
          this.initData() //自定义函数
        }, 20)
      }
    },
  },
  //请求数据
  created() {
    axios.get('/js/data.json').then(
      (response) => {
        this.list = Object.assign([], this.list, response.data.list)
        this.initData()
      },
      (response) => {
        console.log('error')
      }
    )
  },
  mounted() {},

  beforeDestroy(){
    //生命周期销毁时，清楚定时器timer
    clearInterval(this.timer)
    this.timer = null
  }
}
</script>

<style scoped>
.buttonStyle {
  display: block;
}

.fatherCss {
  position: absolute;
}
</style>

