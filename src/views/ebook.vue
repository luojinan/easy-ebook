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
    <ebook-footer 
      :isShow="isShowControl"
      :defaultFontSize="defaultFontSize" 
      @setFontSize="setFontSize"
      :themeList="themeList"
      :defaultTheme="defaultTheme" 
      @setTheme="setTheme"
      :progressAvailabe="Boolean(locations)"
      :defaultProgress="defaultProgress" 
      @changeProgress="changeProgress"
      :navigation="navigation"
      @toPage="toPage"
    />
  </div>
</template>
<script>
import EbookHeader from '@/components/EbookHeader/index'
import EbookFooter from '@/components/EbookFooter/index'
import {ThemeList} from '@/utils/config.js'
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
      locations:'', // 进度对象new Epub(_staticBookUrl).locations
      navigation:{},  // 目录对象new Epub(_staticBookUrl).navigation
      themes:'',
      isShowControl:false,
      defaultFontSize:16,
      themeList:ThemeList,
      defaultTheme:0,  // 默认主题颜色0 1 2 3
      defaultProgress:0
    }
  },
  methods:{
    toPage(item){
      this.isShowControl = false
      this.rendition.display(item.href).then(() => {
        this.updateProgress();
        // 利用then，只有输入进度改变页面，页面改变没有去获取新的进度
      });
    },
    updateProgress() {
      const curLocation = this.rendition.currentLocation()  // 获取当前进度对象
      const percentage = Boolean(this.locations) // 确保而已，实际一定为true
        ? this.locations.percentageFromCfi(curLocation.start.cfi) // 获取进度的具体比例%
        : 0;
      this.defaultProgress = Math.round(percentage * 100); // 进度比例转化为数值
    },
    // 底部设置进度事件
    changeProgress(progress){
      this.defaultProgress = progress //修改底部操作栏样式
      const precentage = progress/100 // 转换为0.xx
      const locationPage = precentage>0?this.locations.cfiFromPercentage(precentage):0  // 某进度dom对象
      this.rendition.display(locationPage)  // 重新渲染dom对象
      
      progress==100&&this.isShowControl&&(this.isShowControl = false)
    },
    // 底部选择字号事件
    setFontSize(fontSize){
      console.log(fontSize);
      this.defaultFontSize = fontSize
      this.themes.fontSize(`${fontSize}px`)
    },
    // 底部选择主题颜色事件
    setTheme(index){
      this.defaultTheme = index
      if (this.themes) {
        this.themes.select(this.themeList[index].name)
      }
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
      // 要操作主题颜色的话，需要先给👆themes实例注册主题颜色列表
      this.registerTheme()
      this.setTheme(0)  // 直接themes.select(主题名)即可,初始化主题颜色

      // 获取locations进度对象（异步）
      book.ready.then(()=>{
        this.navigation = book.navigation // 目录
        return book.locations.generate()  // 进度
      }).then(res=>{
        console.log('异步加载进度完成');
        this.locations = book.locations
      })
    },
    // themes实例注册主题颜色列表
    registerTheme(){
      if(this.themes){
        this.themeList.forEach(item=>{
          this.themes.register(item.name,item.style)
        })
      }
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