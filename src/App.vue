<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
// 利用rem根据最外层根元素的font-size来设置单位的原理
// 让根元素的font-size自适应的话，rem的单位就是自适应的了
document.addEventListener('DOMContentLoaded',()=>{
  const html = document.querySelector('html')
  let fontSize = window.innerWidth/10
  fontSize = fontSize>36?36:fontSize  // 设置1rem最大上限为36px
  html.style.fontSize = `${fontSize}px`
})

</script>

<style lang="scss">
@import "scss/style.scss";
@import 'scss/global';
#app{
  font-size: px2rem(14);
}
</style>
