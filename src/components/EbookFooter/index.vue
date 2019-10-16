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
    }
  },
  data(){
    return {
      isShowSetFont:false
    }
  },
  methods:{

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
    z-index: 101;
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

  .ebook-footer_setter{
    position: absolute;
    left: 0;
    bottom: px2rem(48);
    width: 100%;
    height: px2rem(40);
    background-color: #fff;
    box-shadow: 0 px2rem(-8) px2rem(8) rgba(0, 0, 0, .15);
  }
}
</style>