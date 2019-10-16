<template>
  <div class="ebook">
    <!-- 页面顶部部分 -->
    <ebook-header v-show="isShowControl" />
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
    <ebook-footer :isShow="isShowControl" :defaultFontSize="defaultFontSize" @setFontSize="setFontSize"/>
  </div>
</template>
<script>
import EbookHeader from '@/components/EbookHeader/index'
import EbookFooter from '@/components/EbookFooter/index'
import Epub from 'epubjs' 
const _staticBookUrl = '/static/Wonder.epub'

export default {
  components:{
    EbookHeader,
    EbookFooter
  },
  data(){
    return {
      rendition:'',
      themes:'',
      isShowControl:false,
      defaultFontSize:16
    }
  },
  methods:{
    // 底部选择字号事件
    setFontSize(fontSize){
      console.log(fontSize);
      this.defaultFontSize = fontSize
      this.themes.fontSize(`${fontSize}px`)
    },
    // 点击中间蒙板，显示隐藏上下控制栏，带动画效果
    showControl(){
      console.log('点击显示上下控制栏');
      this.isShowControl = !this.isShowControl
    },
    // 点击左边蒙板，触发ebook类的上一页方法
    prePage(){
      console.log('上一页');
      if(this.rendition) {
        this.rendition.prev()
        this.isShowControl && (this.isShowControl = false)
      }
    },  
    // 点击左边蒙板，触发ebook类的下一页方法
    nextPage(){
      console.log('下一页');
      if(this.rendition) {
        this.rendition.next()
        this.isShowControl && (this.isShowControl = false)
      }
    },
    createdEpub(){
      // 实例化依赖的类（传入电子书资源）
      const book = new Epub(_staticBookUrl)
      // 把电子书生成dom
      this.rendition = book.renderTo('book-reader',{
        width:window.innerWidth,
        height:window.innerHeight
      })
      // 把生成的dom渲染进页面
      this.rendition.display()
      // 获取到实例类的主题操作对象
      this.themes = this.rendition.themes
      this.themes.fontSize(`${this.defaultFontSize}px`)  // 初始化epub显示为自定义字号
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

}
</style>