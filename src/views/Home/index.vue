<template>
  <div class="home-container">
    <van-cell class="title" title="推荐歌单" />
    <van-grid :border="false" :column-num="3">
      <van-grid-item v-for="item in list" :key="item.id">
        <van-image width="100" height="100" :src="item.picUrl" />
        <p class="music-name van-multi-ellipsis--l2">{{ item.name }}</p>
      </van-grid-item>
    </van-grid>

    <van-cell title="最佳匹配" class="title" />
    <van-cell
      :label="`${
        (item.song &&
          item.song.artists &&
          item.song.artists[0] &&
          item.song.artists[0].name) ||
        '未知歌手'
      }-${item.name}`"
      :title="item.name"
      v-for="item in newList"
      :key="item.id"
    >
      <template>
        <van-icon color="#000" name="play-circle-o" size="28" />
      </template>
    </van-cell>
  </div>
</template>

<script>
import { getRecommendListApi, getNewsongListApi } from "@/apis";
import SongItem from "@/components/SongItem.vue";
export default {
  components: {
    SongItem,
  },
  data() {
    return {
      list: [],
      newList: [],
    };
  },
  mounted() {
    this.getList(), this.getNewsongList();
  },
  methods: {
    async getList() {
      try {
        const res = await getRecommendListApi({
          limit: 6,
        });
        this.list = res.data.result;
        // console.log(res.data.result);
      } catch (e) {
        console.log("e", e);
      }
    },
    async getNewsongList() {
      try {
        const res = await getNewsongListApi();
        // console.log(res.data.result);
        this.newList = res.data.result;
      } catch (e) {
        console.log("e", e);
      }
    },
  },
};
</script>

<style lang="less" scoped>
.home-container {
  .title {
    background-color: #dcadd0;
    color: #fff;
    opacity: 0.8;
  }

  .music-name {
    font-size: 12px;
    text-align: left;
    line-height: 20px;
    width: 100%;
    padding: 0 5px;
    box-sizing: border-box;
    height: 37px;
  }

  /deep/.van-grid-item__content {
    padding: 10px 0px;
  }

  .van-cell__value {
    // padding-top: 9px;
    align-self: center;
    flex: 0 0 30px;
  }
}
</style>