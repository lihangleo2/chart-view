<template>
  <div >
    <canvas ref="canvas" width="500" height="500" ></canvas>
  </div>
</template>

<script>
export default {
  //部件
  components: {},
  //静态
  //这里相当于自定义了3个属性
  //lineCenterX: X轴在画布上Y方向的坐标点（这样设计的功能是因为公司业务，看明白了可以把他改掉）
  //textFont: 文字大小
  //value_50: 50μv在画布上的长度
  props: ['lineCenterX', 'textFont','value_50'],
  //对象内部的属性监听，也叫深度监听
  watch: {
    
  },
  //属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
  computed: {},
  //方法表示一个具体的操作，主要书写业务逻辑；
  methods: {
    initDataSon(valueList) {

      //将画布设置满屏
      this.$refs.canvas.width = window.innerWidth
      this.$refs.canvas.height = window.innerHeight
      var ctx = this.$refs.canvas.getContext('2d')

      //当前控件的总长宽
      var total_W = this.$refs.canvas.offsetWidth // 返回元素的总宽度
      var total_H = this.$refs.canvas.offsetHeight // 返回元素的总高度

      ctx.font = 'normal ' + this.textFont + 'px Verdana'
      ctx.fillStyle = '#000000'

      //在画布上确定好+-50μv坐标,并渲染到画布上
      ctx.fillText('-50μv', 14, this.lineCenterX + this.value_50)
      ctx.fillText('+50μv', 14, this.lineCenterX - this.value_50 + this.textFont)


      //横坐标 (先用红线画出X轴)
      ctx.moveTo(0, this.lineCenterX)
      ctx.lineTo(total_W, this.lineCenterX)
      ctx.strokeStyle = '#ff0000'
      ctx.lineWidth = 0.3
      ctx.stroke()

      //纵坐标
      // ctx.moveTo(0, 0)
      // ctx.lineTo(0, total_H)

      //画笔改成黑色，准备画折线图
      ctx.beginPath()
      ctx.strokeStyle = '#000000'
      ctx.lineWidth = 0.6

      
      //填充数据是不断需要更新数据且需要渲染界面，所以这里用到了nextTick方法
      this.$nextTick(() => {

        //每段x轴每个刻度值之间的距离(其实这里的ever_x,是根据你当前坐标轴要展示多长时间的数据算的，比如总共波长是200s,你当前
        //只想展示10s，所以真的项目肯定是确定的，最好是把valueList.length也当成一个属性去传递，是个固定值)
        var ever_x = total_W / (valueList.length - 1)
        for (const key in valueList) {
          
          //将数据放大1000倍(这里完全是根据我们项目走的，可以忽略这点)
          let itemValue = valueList[key] * 1000
          //知道了每个x轴坐标点，那么再计算出每个y轴的坐标点
          let trueItemValue = itemValue/50*this.value_50
          if (key === 0) {
            //单独处理下x=0的点
            ctx.moveTo(0, this.lineCenterX - trueItemValue)
          } else {
            //连接之后的所有点
            ctx.lineTo(ever_x * key, this.lineCenterX - trueItemValue)
          }
        }

        ctx.stroke()

      })


    },
  },
  //请求数据
  created() {},
  mounted() {},


  
}

</script>

<style scoped>

</style>

