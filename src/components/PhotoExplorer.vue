<template>
  <div v-show="visible" class="outside-box">
    <div class="top-bar">
      <div class="title">{{title}}</div>
      <div class="close-btn" v-on:click="close()">
        <img src="@/assets/image/close.svg">
      </div>
    </div>
    <swiper class="mySwiper" ref="mySwiper" :options="swiperOptions">
      <swiper-slide v-for="(photo, index) in photoList" :key="index">
        <div class="image-box">
          <img :src="photo.src">
        </div>
      </swiper-slide>
      <div class="swiper-pagination" slot="pagination"></div>
      <div class="swiper-button-prev swiper-button-white" slot="button-prev"></div>
      <div class="swiper-button-next swiper-button-white" slot="button-next"></div>
    </swiper>
  </div>
</template>

<script>
import { Swiper, SwiperSlide, directive } from 'vue-awesome-swiper';
import 'swiper/css/swiper.css';

export default {
  data() {
    return {
      swiperOptions: {
        // 分頁
        pagination: {
          el: '.swiper-pagination',
          type: 'fraction',
        },
        // 左右切換
        navigation: {
          nextEl: '.swiper-button-next',
          prevEl: '.swiper-button-prev',
          color: 'white',
        },
      },
    };
  },
  props: {
    visible: Boolean, // 是否顯示照片瀏覽器
    title: String, // 標題
    index: Number, // 顯示第幾張照片
    photoList: Array, // 照片列表
  },
  components: {
    Swiper,
    SwiperSlide,
  },
  directives: {
    swiper: directive,
  },
  methods: {
    close() {
      // 關閉瀏覽器，告訴外元件把visible值改成false。
      this.$emit('update:visible', false);
      // Vue內元件沒有辦法直接改props的值，需要用$emit去傳送值到外部元件，但是外部元件要建立一個方法讓內元件呼叫，該方法再修改props的值。
      // 但這樣很麻煩，在vue 2.3後我們可以用update:直接達成效果。
      // (這只是語法上的修飾，底層運作的原理是一模一樣的。)
    },
  },
  watch: {
    visible() {
      // 監聽visible，當瀏覽器顯示時就依照外部傳入的index顯示圖片，並且lock卷軸，反之則解開卷軸。
      if (this.visible) {
        this.swiper.slideTo(this.index, 0, true);
        document.body.style.overflow = 'hidden';
      } else {
        document.body.style.overflow = 'auto';
      }
    },
  },
  computed: {
    swiper() {
      return this.$refs.mySwiper.$swiper;
    },
  },
  mounted() {
  },
};
</script>

<style lang="scss" scoped>
  .outside-box{
    left:0;
    top:0;
    z-index:999;
    position: fixed;
    background-color: rgba($color: black, $alpha: 0.9);
    width: 100vw;
    height: 100vh;
    z-index: 100;
    display: flex;
    flex-direction: column;
    align-items: center;
    .top-bar{
      width: 100%;
      display: flex;
      justify-content: space-between;
      .title{
        padding: 1rem;
        color: white;
      }
      .close-btn{
        padding: 1rem;
        cursor: pointer;
      }
    }

    .mySwiper{
      width: 100%;
      height: 100%;
      margin-bottom: 1rem;

      .swiper-pagination{
        color: white;
        padding: 0.5rem;
        background-color: rgba($color: black, $alpha: 0.8);
        width: auto;
        transform: translate(-50%, -50%);
        border-radius: 6px;
        left: 50%;
      }

      .image-box{
        display: flex;
        align-items: center;
        justify-content: center;
        height: 100%;
        text-align: center;
        img{
          max-width: 100%;
          max-height: 100%;
        }
      }
    }
  }
</style>
