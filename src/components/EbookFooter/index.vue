<template>
<transition name="slide-up">
  <div class="ebook-footer">
    <div class="ebook-footer_icon"><span class="icon-menu icon"></span></div>
    <div class="ebook-footer_icon"><span class="icon-set icon"></span></div>
    <div class="ebook-footer_icon"><span class="icon-bright icon"></span></div>
    <div class="ebook-footer_icon"><span class="icon-a icon">A</span></div>
  </div>
</transition>
</template>
<script>
export default {
  data(){
    return {

    }
  },
  methods:{

  }
}
</script>

<style lang="scss" scoped>
@import '../../scss/global';
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
</style>