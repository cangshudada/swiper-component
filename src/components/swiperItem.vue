<template>
  <transition>
    <div class="swiper-item" v-if="name===selected?true:false">
      <slot></slot>
    </div>
  </transition>
</template>

<script>
export default {
  props: {
    name: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      selected: ""
    };
  },
  computed: {
    isShow() {
      return this.name === this.selected;
    }
  },
  created() {},
  mounted() {},
  watch: {},
  methods: {},
  components: {}
};
</script>

<style scoped lang='less'>
.swiper-item {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
.v-enter-active,
.v-leave-active {
  transition: all 0.6s linear;
  position: absolute;
}
.v-leave-to {
  transform: translate(-100%);
}
.v-enter {
  transform: translate(100%);
}
</style>
