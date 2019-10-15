<template>
  <div class="ebook">
    <!-- 页面顶部部分 -->
    <transition name="slide-down">
      <div class="ebook-header" v-show="isShowControl">
        <div class="ebook-header_left"><span class="icon-back icon"></span></div>
        <div class="ebook-header_right">
          <div class="ebook-header_right--icon"><span class="icon-cart icon"></span></div>
          <div class="ebook-header_right--icon"><span class="icon-person icon"></span></div>
          <div class="ebook-header_right--icon"><span class="icon-more icon"></span></div>
        </div>
      </div>
    </transition>
    <!-- 阅读器主体部分 -->
    <div class="ebook-wrapper">
      <div id="book-reader"></div>
      <div class="ebook-mask">
        <div class="ebook-mask_left" @click="prePage"></div>
        <div class="ebook-mask_center" @click="showControl"></div>        
        <div class="ebook-mask_right" @click="nextPage"></div>
      </div>
    </div>
    <!-- 页面底部部分 -->
    <transition name="slide-up">
      <div class="ebook-footer" v-show="isShowControl">
        <div class="ebook-footer_icon"><span class="icon-menu icon"></span></div>
        <div class="ebook-footer_icon"><span class="icon-set icon"></span></div>
        <div class="ebook-footer_icon"><span class="icon-bright icon"></span></div>
        <div class="ebook-footer_icon"><span class="icon-a icon">A</span></div>
      </div>
    </transition>
  </div>
</template>
<script>
import Epub from 'epubjs' 
const _staticBookUrl = '/static/Wonder.epub'
export default {
  data(){
    return {
      rendition:'',
      isShowControl:false
    }
  },
  methods:{
    // 点击中间蒙板，显示隐藏上下控制栏，带动画效果
    showControl(){
      console.log('点击显示上下控制栏');
      this.isShowControl = !this.isShowControl
    },
    // 点击左边蒙板，触发ebook类的上一页方法
    prePage(){
      console.log('上一页');
      if(this.rendition) this.rendition.prev()
    },  
    // 点击左边蒙板，触发ebook类的下一页方法
    nextPage(){
      console.log('下一页');
      if(this.rendition) this.rendition.next()
    },
    createdEpub(){
      // 实例化依赖的类（传入电子书资源）
      const book = new Epub(_staticBookUrl)
      console.log(book)
      // 把电子书生成dom
      this.rendition = book.renderTo('book-reader',{
        width:window.innerWidth,
        height:window.innerHeight
      })
      // 把生成的dom渲染进页面
      this.rendition.display()

    }
  },
  mounted(){
    this.createdEpub()
  }
}
</script>

<style lang="scss" scoped>
@import '../scss/global';
.ebook{
  position: relative;
  .ebook-header{
    display: flex;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: px2rem(48);
    background-color: #fff;
    z-index: 101;
    box-shadow: 0 px2rem(8) px2rem(8) rgba(0, 0, 0, .15);
    .ebook-header_left{
      flex: 0 0 px2rem(40);
      @include center;
      .icon-back{
        font-size: px2rem(18)
      }
    }
    .ebook-header_right{
      flex: 1;
      display: flex;
      justify-content: flex-end;  // 只写end是无效的
      .ebook-header_right--icon{
        flex: 0 0 px2rem(40);
        @include center;
      }
    }
  }

  .ebook-wrapper{
    .ebook-mask{
      display: flex;
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: 100;
      .ebook-mask_left{
        flex: 0 0 px2rem(100);
      }
      .ebook-mask_center{
        flex: 1;
      }
      .ebook-mask_right{
        flex: 0 0 px2rem(100);
      }
    }
  }

  .ebook-footer{
    display: flex;
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: px2rem(44);
    z-index: 101;
    background-color: #fff;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
    .ebook-footer_icon{
      flex: 1;
      @include center;
      .icon-set,.icon-bright{
        font-size: px2rem(26);
      }
    }
  }
}
</style>