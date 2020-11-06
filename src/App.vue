<template>
  <div id="app">
    <PhotoExplorer :title="photoExplorerTitle" :photoList="photoList"
    :index="photoExplorerIndex" :visible.sync="photoExplorerVisible"></PhotoExplorer>
    <!-- 利用.sync修飾語法來雙向綁定內外元件的visible值，這樣就不用像2.3之前，再寫一個method來改變值。 -->
    <div class="container">
      <div class="photoListBox">
        <ul class="row">
          <li class="col-12 col-md-6 col-lg-4 col-xl-3"
          v-for="(photo, index) in photoList" :key="index"
          v-on:click="openImageExplorer(index)">
            <div class="photo" :style="{backgroundImage: 'url(' + photo.src + ')'}"></div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script>
import PhotoExplorer from '@/components/PhotoExplorer.vue';

export default {
  name: 'App',
  data() {
    return {
      // PhotoExplorer參數
      photoExplorerTitle: '照片瀏覽', // 瀏覽器標題
      photoExplorerVisible: false, // PhotoExplorer顯示狀態
      photoExplorerIndex: 1, // PhotoExplorer顯示第幾張照片
      // 照片列表
      photoList: [
        {
          src: 'https://placehold.it/600x1500',
        },
        {
          src: 'https://placehold.it/200x100',
        },
        {
          src: 'https://placehold.it/600x800',
        },
        {
          src: 'https://placehold.it/800x600',
        },
        {
          src: 'https://placehold.it/600x800',
        },
        {
          src: 'https://placehold.it/800x600',
        },
        {
          src: 'https://placehold.it/600x800',
        },
        {
          src: 'https://placehold.it/800x600',
        },
      ],
    };
  },
  components: {
    PhotoExplorer,
  },
  methods: {
    // 開啟瀏覽器，傳入要從第幾張照片開始顯示。
    openImageExplorer(index) {
      this.photoExplorerIndex = index;
      this.photoExplorerVisible = true;
    },
  },
};
</script>

<style lang="scss" src="bootstrap/scss/bootstrap.scss" scoped></style>
<style lang="scss" scoped>
  .photoListBox{
    ul{
      list-style: none;
      padding-left: 0;
      margin-bottom: 0;
      li{
        margin: 1rem 0;
        .photo{
          //讓圖片長寬比為1:1
          width: 100%;
          padding-bottom: 100%;
          background-size: cover;
          background-position: center;
          background-repeat: no-repeat;
          cursor: pointer;
        }
      }
    }
  }
</style>
