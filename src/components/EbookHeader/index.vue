<template>
<transition name="slide-down">
  <div class="ebook-header">
    <div class="ebook-header_left"><span class="icon-back icon"></span></div>
    <div class="ebook-header_right">
      <div class="ebook-header_right--icon"><span class="icon-cart icon"></span></div>
      <div class="ebook-header_right--icon"><span class="icon-person icon"></span></div>
      <div class="ebook-header_right--icon"><span class="icon-more icon"></span></div>
    </div>
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
</style>