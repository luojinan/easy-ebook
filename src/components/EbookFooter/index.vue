<template>
<div class="ebook-footer_wrapper">
  <transition name="slide-up">
    <div class="ebook-footer" v-show="isShow" :class="{'hide-box-shadow':isShowSetFont||!isShow}">
      <div class="ebook-footer_icon"><span class="icon-menu icon"></span></div>
      <div class="ebook-footer_icon"><span class="icon-set icon"></span></div>
      <div class="ebook-footer_icon"><span class="icon-bright icon"></span></div>
      <div class="ebook-footer_icon"><span class="icon-a icon" @click="isShowSetFont=!isShowSetFont">A</span></div>
    </div>
  </transition>
  <transition name="slide-up">
    <div class="ebook-footer_setter" v-show="isShowSetFont">
      <div class="setter-fontsize">
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
    }
  },
  data(){
    return {
      isShowSetFont:false
    }
  },
  methods:{
    setFontSize(fontSize){
      this.$emit('setFontSize',fontSize)
    }
  },
  watch:{
    isShow(newVal){
      if(!newVal) this.isShowSetFont = false
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../../scss/global';
.ebook-footer_wrapper{
  .ebook-footer{
    display: flex;
    position: absolute;
    bottom: 0;
    left: 0;
    width: 100%;
    height: px2rem(44);
    z-index: 102;
    background-color: #fff;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
    &.hide-box-shadow{
      box-shadow: none;
    }
    .ebook-footer_icon{
      flex: 1;
      @include center;
      .icon-set,.icon-bright{
        font-size: px2rem(26);
      }
    }
  }
  // 字号选择容器部分
  .ebook-footer_setter{
    position: absolute;
    left: 0;
    bottom: px2rem(44);
    width: 100%;
    height: px2rem(40);
    z-index: 101;
    background-color: #fff;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
    .setter-fontsize{
      display: flex;
      height: 100%;
      // 字号选择，左右A预览部分
      .setter-fontsize_preview{
        flex: 0 0 px2rem(40);
        @include center;
      }
      .setter-fontsize_for--firstchild{
        flex: 1;
        display: flex;
        .setter-fonsize_select{
          flex: 1;
          display: flex;
          &:first-child{
            .selected-line{
              &:first-child{
                // background-color: red;
                border: none;
              }
            }
          }
          &:last-child{
            .selected-line{
              &:last-child{
                // background-color: red;
                border: none;
              }
            }
          }
          .setter-fontsize_selected--warapper{
            flex: 1;
            display: flex;
            align-items: center;
            // 字号选择横线样式
            .selected-line{
              flex: 1;
              height: 0;
              border-top: px2rem(1) solid #ccc;
            }
            // 字号选择竖线样式
            .selected-point_wrapper{
              position: relative;
              flex: 0 0 0;
              width: 0;
              height: px2rem(7);
              border-left: px2rem(1) solid #ccc;
              // 字号选择圆形样式
              .selected-point{
                position: absolute;
                top: px2rem(-7);
                left: px2rem(-10);
                width: px2rem(18);
                height: px2rem(18);
                border-radius: 50%;
                background-color: #fff;
                border: px2rem(1) solid #ccc;
                box-shadow: 0 px2rem(4) px2rem(4) rgba(0, 0, 0, .15);
                @include center;
                .selected-point_brackpoint{
                  width: px2rem(5);
                  height: px2rem(5);
                  background-color: #000;
                  border-radius: 50%;
                }
              }
            }
          }
        }

      }
    }
  }
}
</style>