<template>
<div class="ebook-footer_wrapper">
  <!-- 底部操作栏部分 -->
  <transition name="slide-up">
    <div class="ebook-footer" v-show="isShow" :class="{'hide-box-shadow':isShowSet||!isShow}">
      <div class="ebook-footer_icon"><span class="icon-menu icon"></span></div>
      <div class="ebook-footer_icon"><span class="icon-set icon" @click="showSet(2)"></span></div>
      <div class="ebook-footer_icon"><span class="icon-bright icon" @click="showSet(3)"></span></div>
      <div class="ebook-footer_icon"><span class="icon-a icon" @click="showSet(4)">A</span></div>
    </div>
  </transition>
  <!-- 设置部分 -->
  <transition name="slide-up">
    <div class="ebook-footer_setter" v-show="isShowSet">
      <!-- 设置字号大小部分 -->
      <div class="setter-fontsize" v-show="setType == 4">
        <!-- 左边最小字号预览 -->
        <div class="setter-fontsize_preview" :style="{fontSize:`${fontSizeList[0]}px`}">A</div>
        <!-- 中间线条选择字号 -->
        <div class="setter-fontsize_for--firstchild">
          <div class="setter-fonsize_select" v-for="(item,index) in fontSizeList" :key="index">
            <!-- 每一段为 ' —O— ' -->
            <div class="setter-fontsize_selected--warapper">
              <!-- 左横线 -->
              <div class="selected-line"></div>
              <!-- 竖线部分 -->
              <div class="selected-point_wrapper" @click="setFontSize(item)">
                <!-- 圆形部分 -->
                <div class="selected-point" v-show="defaultFontSize==item">
                  <div class="selected-point_brackpoint"></div>
                </div>
              </div>
              <!-- 右横线 -->
              <div class="selected-line"></div>
            </div>
          </div>
        </div>
        <!-- 右边最大字号预览 -->
        <div class="setter-fontsize_preview" :style="{fontSize:`${fontSizeList[fontSizeList.length-1]}px`}">A</div>
      </div>
      <!-- 设置主题颜色部分 -->
      <div class="ebook-footer_theme" v-show="setType == 3">
        <div
          class="ebook-footer_theme--item" 
          v-for="(item,index) in themeList" 
          :key="index"
          @click="setTheme(index)"
        >
          <div 
            class="theme-item_preview" 
            :class="{'no-border':item.style.body.background!='#fff'}"
            :style="{'background':item.style.body.background}
          "></div>
          <p :class="{'selected-theme':defaultTheme==index}">{{item.name}}</p>
        </div>
      </div>
      <!-- 设置进度部分 -->
      <div class="setter-progress" v-show="setType == 2">
        <div class="setter-progress_wrapper" v-if="progressAvailabe">
          <input 
            type="range" 
            max="100" 
            min="0"
            step="1"
            @change="progressChange"
            @input="progressInput"
            :value="defaultProgress"
            :disabled="!progressAvailabe"
            ref="progress">{{defaultProgress+'%'}}
        </div>
        <p v-else>加载中...</p>
      </div>
    </div>
  </transition>

</div>
</template>
<script>
export default {
  props:{
    isShow:{
      type:Boolean,
      default:false
    },
    fontSizeList:{
      type:Array,
      default:()=>[14,16,18,20,22,24]
    },
    defaultFontSize:{
      type:[String,Number],
      default:16
    },
    themeList:{
      type:Array,
      default:()=>[]
    },
    defaultTheme:{
      type:[String,Number],
      default:0
    },
    progressAvailabe:{
      type:Boolean,
      default:false
    },
    defaultProgress:{
      type:[String,Number],
      default:10
    }
  },
  data(){
    return {
      isShowSet:false,  // 设置部分(不是底部操作栏)的显示隐藏
      setType:''  // 设置部分1-目录 2-进度 3-主题 4-字号
    }
  },
  methods:{
    // 拖动进度条时触发的事件
    progressChange(e){
      // js动态修改dom的样式
      this.$refs.progress.style.backgroundSize = `${e.target.value}% 100%`
    },
    // 松开进度条时触发的事件
    progressInput(e){
      this.$emit('changeProgress',e.target.value)
    },
    // 选中主题颜色
    setTheme(index){
      this.$emit('setTheme',index)
    },
    // 选中字号
    setFontSize(fontSize){
      this.$emit('setFontSize',fontSize)
    },
    // 切换底部操作栏-同时显示相应设置部分
    showSet(type){
      if(!this.isShowSet) {
        this.setType = type
        this.isShowSet = true
        return
      }
      if(this.isShowSet&&(!this.setType||this.setType == type)) this.isShowSet = false
      this.setType = type
    }
  },
  watch:{
    // 监听页面显示隐藏底部操作
    isShow(newVal){
      if(!newVal) this.isShowSet = false
    }
  }
}
</script>

<style lang="scss" scoped>
@import '/index';
</style>