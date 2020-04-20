<template>
  <div class="piture_intervew">
    <Loading v-if="!isPitureServerLoad" />
    <swiper v-model="selName" 
      isShowArrow 
      isHoverPause 
      autoplay 
      interval="2500" 
      v-else
    >
      <swiper-Item class="piture" :name="item.name" v-for="(item,index) in picture" :key="index">
        <img :title='item.name' :src="item.picture" :alt="item.unit" />
      </swiper-Item>
    </swiper>
  </div>
</template>

<script>
import Loading from "./components/loading";
import Swiper from "./components/swiper";
import SwiperItem from "./components/swiperItem";
export default {
  props: {},
  data() {
    return {
      isPitureServerLoad: false,
      // 选中显示项
      selName: "大同地区煤炭勘察报告",
      picture: []
    };
  },
  computed: {},
  created() {
    this.getServerPicture();
  },
  mounted() {},
  watch: {},
  methods: {
    // 获得图片接口数据
    getServerPicture() {
      this.$axios
        .get("http://101.200.84.173:59087/roll/query?limit=5")
        .then(res => {
          if (res.data.flag === "true") {
            this.picture = res.data.data.map(item => {
              item.picture = `data:image/png;base64,${item.picture}`;
              return item;
            });
            this.isPitureServerLoad = true;
          } else {
            throw new Error("图片请求失败");
          }
        })
        .catch(error => {
          throw new Error("图片请求失败");
        });
    }
  },
  components: {
    Loading,
    Swiper,
    SwiperItem
  }
};
</script>

<style scoped lang='less'>
.piture_intervew {
  position: relative;
  width: 100%;
  height: 100vh;
  .swiper {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 100%;
    height: 560px;
    .piture {
      width: 100%;
      height: 100%;
    }
  }
}
</style>

