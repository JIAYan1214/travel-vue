<template>
  <div>
    <home-header></home-header>
    <home-swiper :list="swiperList"></home-swiper>
    <home-icons :list="iconList"></home-icons>
    <home-recommend :list="recommendList"></home-recommend>
    <home-weekend :list="weekendList"></home-weekend>
  </div>
</template>

<script>
import HomeHeader from "./components/Header.vue";
import HomeSwiper from "./components/Swiper.vue";
import HomeIcons from "./components/Icons.vue";
import HomeRecommend from "./components/Recommend.vue";
import HomeWeekend from "./components/Weekend.vue";
import axios from "axios";
import { mapState } from "vuex";
export default {
  name: "Home",
  components: {
    HomeHeader,
    HomeSwiper,
    HomeIcons,
    HomeRecommend,
    HomeWeekend
  },
  data() {
    return {
      swiperList: [],
      iconList: [],
      recommendList: [],
      weekendList: [],
      lastCity: ""
    };
  },
  computed: {
    ...mapState(["city"])
  },
  methods: {
    getHomeInfo() {
      axios
        .get(`/api/home.json?city=${this.city}`)
        .then(this.getHomeInfoSuccss);
    },
    getHomeInfoSuccss(response) {
      const res = response.data;
      const data = res.data;
      if (res.ret && res.data) {
        this.swiperList = data.swiperList;
        this.iconList = data.iconList;
        this.recommendList = data.recommendList;
        this.weekendList = data.weekendList;
      }
    }
  },
  mounted() {
    this.getHomeInfo();
    this.lastCity = this.city; // lastCity临时缓冲变量
  },
  activated() { // Home组件激活(回到home主页)时调用
    if (this.city !== this.lastCity) {
      this.getHomeInfo();
      this.lastCity = this.city;
    }
  }
};
</script>

<style>
</style>
