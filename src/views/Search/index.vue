<template>
  <div>
    <van-search
      shape="round"
      placeholder="请输入搜索关键词"
      v-model="value"
      @input="inputFn"
    />
    <!-- 热门搜索 -->
    <template v-if="searchList.length == 0">
      <van-cell title="热门搜索" />
      <div style="padding: 10px 16px">
        <van-tag
          color="#4eacb4"
          text-color="#dcaddd"
          size="large"
          plain
          round
          type="primary"
          v-for="(item, index) in tags"
          :key="index"
          @click="clickFn(item.first)"
          style="margin-right: 6px"
        >
          {{ item.first }}
        </van-tag>
      </div>
    </template>

    <template v-else>
      <van-cell title="最佳匹配" />

      <van-list
        v-model="loading"
        :finished="finished"
        finished-text="没有更多了"
        @load="onLoad"
      >
        <van-cell
          v-for="item in searchList"
          :key="item.id"
          :title="item.name"
          :label="`${item.ar[0].name}-${item.name}`"
        />
      </van-list>
    </template>
  </div>
</template>
<script>
export default {};
</script>

<script>
import { getSearchTagApi, getSearchListApi } from "@/apis";
export default {
  data() {
    return {
      tags: [],
      value: "",
      searchList: [],
      loading: false,
      finished: false,
      page: 1,
    };
  },
  created() {
    this.getTagList();
  },
  methods: {
    async getListFn() {
      return await getSearchListApi({
        keywords: this.value,
        limit: 20,
        offset: (this.page - 1) * 20,
      });
    },
    async getTagList() {
      try {
        const res = await getSearchTagApi();
        // console.log(res);
        this.tags = res.data.result.hots;
      } catch (e) {
        console.log("e", e);
      }
    },
    async clickFn(val) {
      this.page = 1;
      this.finished = false;
      this.value = val;
      try {
        const res = await getSearchListApi({
          keywords: this.value,
        });
        //   console.log(res.data.result.songs);
        this.searchList = res.data.result.songs;
        this.loading = false;
        this.$toast.success("搜索成功");
      } catch (e) {
        this.$toast.fail("搜索失败");
      }
    },
    async onLoad() {
      this.page++;
      const res = await this.getListFn();
      if (res.data.result.songs === undefined) {
        this.finished = true;
        this.loading = false;
        return;
      }
      this.searchList = [...this.searchList, ...res.data.result.songs];
      this.loading = false;
    },
    async inputFn() {
      if (this.timer) clearTimeout(this.timer);
      this.timer = setTimeout(async () => {
        this.page = 1;
        this.finished = false;
        if (this.value.length == 0) {
          this.searchList = [];
          return;
        }
        const res = await this.getListFn();
        //   console.log(res);
        if (res.data.result.songs === undefined) {
          this.searchList = [];
          return;
        }
        this.searchList = res.data.result.songs;
        this.loading = false;
      }, 900);
    },
  },
};
</script>

<style>
</style>