<template>
  <div class="m-header">
    <h1 class="text">网易云音乐</h1>
    <router-link to="/user" class="mine" tag="div">
      <i class="iconfont icon-list"></i>
    </router-link>
    <router-link to="/search" class="search" tag="div">
      <i class="iconfont icon-search"></i>
    </router-link>
  </div>
</template>

<script>
export default {};
</script>

<style lang="stylus" scoped>
@import "../../common/stylus/variable"
@import "../../common/stylus/mixin"

 .m-header
    position: relative
    height: 44px
    text-align: center
    color: $color-theme
    font-size: 0
    .text
      display inline-block
      vertical-align: top
      line-height: 44px
      font-size: $font-size-large
    .mine
      position absolute
      top -1.5px
      left 0
      .iconfont
        display: block
        padding: 12px
        font-size: 20px
        color: $color-theme-l
    .search
        position absolute
        top 0
        right 2px
        .iconfont
            display block
            padding 12px
            font-size 18px
            color $color-theme-l
</style>