<template>
  <div id="app">
    <router-view />
  </div>
</template>


<script>
export default {
//部件
 components: {
 },
//静态
 props: {
 },
//对象内部的属性监听，也叫深度监听
 watch: {
 },
//属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用；
 computed: { 
 },
//方法表示一个具体的操作，主要书写业务逻辑；
 methods: { 
 },
//请求数据
 created() {
},
 mounted() {
}
}
</script>




<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>

