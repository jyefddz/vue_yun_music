<template>
  <div style="padding-bottom: 48px">
    <van-nav-bar :title="$route.meta.title" style="background-color: #5fcddc" />
    <router-view></router-view>
    <van-tabbar route>
      <van-tabbar-item
        icon="home-o"
        to="/layout/home"
        style="background-color: #5fcddc"
        >首页</van-tabbar-item
      >
      <van-tabbar-item
        icon="search"
        to="/layout/search"
        style="background-color: #5fcddc"
        >搜索</van-tabbar-item
      >
    </van-tabbar>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
};
</script>

<style>
.main {
  padding-top: 46px;
  padding-bottom: 50px;
}
</style>