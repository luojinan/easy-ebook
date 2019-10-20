<template>
  <transition name="slide-right">
    <div class="toc-content">
      <div class="toc-content_wrapper" v-if="tocAvailable">
        <div 
          class="toc-content_item"
          v-for="(item,index) in navigation.toc"
          :key="index"  
          @click="toPage(item)"
        >
          <span>{{item.label}}</span>
        </div>
      </div>
      <p v-else>加载中...</p>
    </div>
  </transition>
</template>
<script>
export default {
  props:{
    navigation:{
      type:Object,
      default:()=>{}
    },
    tocAvailable:{
      type:Boolean,
      default:false
    }
  },
  methods:{
    toPage(item){
      console.log('跳转到页面',item);
      this.$emit('toPage',item)
    }
  }
}
</script>
<style scoped lang="scss">
@import '../../scss/global.scss';
.toc-content{
  position: fixed;
  left: 0;
  top: 0;
  bottom: px2rem(44);
  width: 70%;
  z-index: 104;
  background: #fff;
  @include center;
  .toc-content_wrapper{
    width: 100%;
    height: 100%;
    overflow: auto; // 超出部分加滚动条
    .toc-content_item{
      border-bottom: px2rem(1) solid #f4f4f4;
      padding: px2rem(20) px2rem(20);
      span{
        font-size: px2rem(14);
        color: #333;
      }
    }
    p{
      @include center;
      font-size: px2rem(16);
      color: #333;
    }
  }
}

</style>