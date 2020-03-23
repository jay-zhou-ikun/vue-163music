<template>
  <div class="recomend" ref="recommend">
    <div v-show="banner.length" class="decorate" v-if="banner.length"></div>
    <div v-if="banner.length" class="slider-wrapper">
      <slider>
        <div v-for="item in banner" :key="item.id">
          <img :src="item.imageUrl" />
        </div>
      </slider>
    </div>
    <!-- <div class="recommend-list" ref="recommendList">
      <h1 class="list-title">热门歌单推荐</h1>
      <ul>
        <li class="item" v-for="item in playList" :key="item.id">
          <div class="icon">
            <img :src="item.picUrl" width="60" height="60" />
          </div>
          <div class="text">
            <h2 class="name" v-html="item.name">{{item.name}}</h2>
            <p class="desc" v-html="item.copywriter"></p>
          </div>
        </li>
      </ul>
    </div> -->
  </div>
</template>

<script>
import Slider from '../../base/slider/slider'
import {getBanner, getRecommendList} from '../../api/recommend'
import {ERR_OK} from '../../common/js/config'
export default {
  data() {
    return {
      banner: [],
      playList:[],
      // recommendMusic: []

    }
  },
  created() {
    this._getBanner()
    // this._getRecommendList()
    // this._getRecommendMusic()
  },
  methods: {
    _getBanner() {
      getBanner().then((res) => {
        if (res.status === ERR_OK) {
          let list = res.data.banners
          this.banner = list.splice(5)
          console.log(this.banner)
        } else {
          console.log('Banner 获取失败')
        }
      })
    },
    // _getRecommendList () {
    //   getRecommendList().then((res) => {
    //     if (res.status === ERR_OK) {
    //       this.playList = res.data.result
    //       console.log(this.playList)
    //     } else {
    //       console.error('getRecommendList 获取失败')
    //     }
    //   })
    // },
    //  _getRecommendMusic () {
    //   getRecommendMusic().then((res) => {
    //     if (res.status === ERR_OK) {
    //       let list = res.data.result.map((item) => {
    //         return createRecommendSong(item)
    //       })
    //       list.splice(9)
    //       this.recommendMusic = list
    //     } else {
    //       console.error('getRecommendMusic 获取失败')
    //     }
    //   })
  },
  components: {
    Slider
  }
}
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/variable';
@import '../../common/stylus/mixin';

.recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0;
    .recommend-content
      height: 100%
      overflow: hidden
      .slider-wrapper
        position: relative
        width: 100%
        overflow: hidden
      .recommend-list
        .list-title
          height: 65px
          line-height: 65px
          text-align: center
          font-size: $font-size-medium
          color: $color-theme
        .item
          display: flex
          box-sizing: border-box
          align-items: center
          padding: 0 20px 20px 20px
          .icon
            flex: 0 0 60px
            width: 60px
            padding-right: 20px
          .text
            display: flex
            flex-direction: column
            justify-content: center
            flex: 1
            line-height: 20px
            overflow: hidden
            font-size: $font-size-medium
            .name
              margin-bottom: 10px
              color: $color-text
            .desc
              color: $color-text-d
</style>