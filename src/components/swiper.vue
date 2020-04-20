<template>
  <div class="swiper" @mouseenter="mouseover" @mouseleave="mouseLeave">
    <div class="viewport">
      <slot></slot>
    </div>
    <!-- 下方指示器 -->
    <div class="swiper_indicator">
      <div
        :class="['swiper_indicator-button',
        value === item ? 'isSel': ''
        ]"
        v-for="(item,index) in names"
        :key="index"
        @click="selPiture(item)"
      ></div>
    </div>
    <!-- 两侧指示器 -->
    <div class="swiper_arrow" v-if="isShowArrow">
      <div class="swiper-arrow swiper_arrow-left" @click="showLeft"></div>
      <div class="swiper-arrow swiper_arrow-right" @click="showRight"></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // v-model
    value: {
      type: String,
      default: ""
    },
    // 是否自动轮播
    autoplay: {
      type: Boolean,
      default: true
    },
    // 轮播时间
    interval: {
      type: String,
      default: "2000"
    },
    // 是否hover暂停
    isHoverPause: {
      type: Boolean,
      default: true
    },
    isShowArrow: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      timer: null,
      selName: "",
      names: null
    };
  },
  computed: {},
  created() {},
  mounted() {
    this.names = this.$children.map(vm => vm.name);
    this.showPiture();
    this.autoShow();
  },
  watch: {
    value(newVal) {
      this.showPiture();
    }
  },
  methods: {
    // 显示
    showPiture() {
      // 设置显示的轮播页 如果没有默认第一个
      this.selName = this.value || this.$children[0].name;
      this.$children.forEach(vm => {
        vm.selected = this.selName;
      });
    },
    // 选择显示
    selPiture(name) {
      this.$emit("input", name);
    },
    // 自动轮播
    autoShow() {
      if (this.autoplay) {
        this.timer = setInterval(() => {
          this.updateShow();
        }, parseInt(this.interval));
      }
    },
    // 轮播索引切换
    updateShow(isLeft) {
      // 自动轮播
      let index = this.names.indexOf(this.selName);
      let newIndex;
      if (isLeft) {
        newIndex = --index;
      } else {
        newIndex = ++index;
      }
      if (newIndex === this.names.length) {
        newIndex = 0;
      }
      if (newIndex === -1) newIndex = this.names.length - 1;
      // 改变v-model
      this.$emit("input", this.names[newIndex]);
    },
    // 鼠标移入
    mouseover() {
      if (this.isHoverPause) {
        this.clearTimer();
      }
    },
    // 鼠标移出
    mouseLeave() {
      if (this.isHoverPause) {
        this.autoShow();
      }
    },
    // 是否暂停轮播 --- 清除定时器
    clearTimer() {
      if (this.timer) {
        clearInterval(this.timer);
      }
    },
    // 显示左边轮播图
    showLeft() {
      this.updateShow(true);
    },
    // 显示右边轮播图
    showRight() {
      this.updateShow();
    }
  },
  beforeDestroy() {
    this.clearTimer();
  },
  components: {}
};
</script>

<style scoped lang='less'>
.swiper {
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
  .viewport {
    width: 100%;
    height: 100%;
  }
  .swiper_indicator {
    position: absolute;
    left: 50%;
    bottom: 20px;
    transform: translateX(-50%);
    display: flex;
    justify-items: center;
    .swiper_indicator-button {
      width: 42px;
      height: 4px;
      background-color: rgb(211, 220, 230);
      cursor: pointer;
      border-radius: 2px;
      & + .swiper_indicator-button {
        margin-left: 12px;
      }
      &.isSel {
        background-color: rgb(153, 169, 191);
      }
    }
  }
  .swiper-arrow {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    border: none;
    outline: none;
    height: 44px;
    width: 44px;
    cursor: pointer;
    transition: 0.3s;
    border-radius: 50%;
    color: #fff;
    z-index: 10;
    text-align: center;
    font-size: 12px;
    &.swiper_arrow-left {
      left: 30px;
      background: url("../assets/img/arrow_left.svg") no-repeat center;
      background-size: 50%;
      background-color: rgb(125, 140, 161);
    }
    &.swiper_arrow-right {
      right: 30px;
      background: url("../assets/img/arrow_right.svg") no-repeat center;
      background-size: 50%;
      background-color: rgb(125, 140, 161);
    }
    &:hover {
      background-color: rgb(153, 169, 191);
    }
  }
}
</style>
