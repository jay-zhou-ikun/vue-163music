<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot>
      </slot>
    </div>
    <div class="dots">
      <span
        class="dot"
        v-for="(item, index) in dots"
        :key="index"
        :class="{'active' : currentPageIndex === index}"
      ></span>
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll";
import { addClass } from "../../common/js/dom";
export default {
  data() {
    return {
      dots: [],
      currentPageIndex: 0
    };
  },
  props: {
    loop: {
      type: Boolean,
      default: true
    },
    autoPlay: {
      type: Boolean,
      default: true
    },
    interval: {
      type: Number,
      default: 4000
    }
  },
  mounted() {
    setTimeout(() => {
      this._setSliderWidth(); // 横向滚动，要先设置slider的宽度
      this._initDots(); // 就那个小点,因为是自动轮播，BScroll会自动在前后加一个sliderGroup，为了保持数量一直，要在BScroll之前初始化
      this._initSlider(); // 初始化BetterScroll在mounted里，等dom已经ready的时候
      this._onScrollEnd()
     }, 20); // 浏览器的刷新通常是17毫秒一次，初始化操作放在20毫秒后，比较保险

  window.addEventListener('resize', () => {
      if (!this.slider) {
        return
      }
      this._setSliderWidth(true)
    })
  },
  methods: {
    _setSliderWidth() {
      this.children = this.$refs.sliderGroup.children;
      //   console.log(this.children.length)
      let width = 0;
      let sliderWidth = this.$refs.slider.clientWidth;
      for (let i = 0; i < this.children.length; i++) {
        const child = this.children[i];
        addClass(child, "slider-item");
        child.style.width = sliderWidth + "px";
        width += sliderWidth;
      }

      if (this.loop) {
        width += 2 * sliderWidth;
      }
      this.$refs.sliderGroup.style.width = width + "px";
    },

    _initSlider() {
      this.slider = new BScroll(this.$refs.slider, {
        scrollX: true, // 横向滚动
        scrollY: false, // 不允许纵向滚动
        momentum: false, // 当快速滑动时是否开启滑动惯性
        snap: {
          loop: this.loop, // 是否可以无缝循环轮播
          threshold: 0.3, // 手动切换时的阈值
          speed: 400
        }
      });
      this.slider.on("scrollEnd", this._onScrollEnd);
    },
    _onScrollEnd() {
      let pageIndex = this.slider.getCurrentPage().pageX;
      this.currentPageIndex = pageIndex;
      if (this.autoPlay) {
        this._play();
      }
    },
    _play() {
      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.slider.next();
      }, this.interval);
    },
    _initDots() {
      this.dots = new Array(this.children.length);
    }
  },
  destroyed() {
    clearTimeout(this.timer);
  }
};
</script>

<style lang="stylus" scoped>
@import "../../common/stylus/variable";
.slider {
  min-height: 1px;
  position: relative;
  .slider-group {
    position: relative;
    overflow: hidden;
    white-space: nowrap;
    .slider-item {
      float: left;
      box-sizing: border-box;
      overflow: hidden;
      text-align: center;
      img {
        display: block;;
        width: 100%
      }
    }
  }
  .dots {
    position: absolute;
    right: 0;
    left: 0;
    bottom: 12px;
    text-align: center;
    font-size: 0;
    .dot {
      display: inline-block;
      margin: 0 4px;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: $color-text-l;
      &.active {
        width: 10px;
        border-radius: 5px;
        background: $color-highlight-background;
      }
    }
  }
}
</style>