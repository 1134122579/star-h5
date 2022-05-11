<template>
  <div class="detailpage">
    <div class="radius">asd</div>
    <div class="title">来自天空之橙·Design | 空间设计</div>
    <div class="label">
      <p>建筑·空间·设计 的主题</p>
      <p class="time">2022/5/7/ 15:41</p>
    </div>
    <div class="content-text">
      来自天空之橙·Design来自天空之橙·Design来自天空之橙·Design来自天空之橙·Design来自天空之橙·Design来自天空之橙·Design来自天空之橙·Design
    </div>
    <div class="content-imglist">
      <div class="imgstyle" v-for="index in 8"></div>
    </div>
  </div>
</template>

<script>
import { getShareArticleDetails } from '@/api/user'
export default {
  data() {
    return {}
  },
  created() {
    this.getList()
  },
  methods: {
    getList() {
      let article_id = this.$route.query.id
      getShareArticleDetails({ article_id }).then(res => {
        this.detail = res.data
      })
    }
  }
}
</script>
<style lang="scss">
.detailpage {
  padding: 30px 20px;
  position: relative;
  background: rgba(0, 0, 0, 0);
  //   圆
  .radius {
    border-radius: 50%;
    width: 150px;
    height: 150px;
    overflow: hidden;
    position: absolute;
    top: -75px;
    right: -75px;
    background: radial-gradient(circle, #10bb9b, #95dbd1, #10bb9b);
  }
  .title {
    font-size: 22px;
    font-weight: 600;
  }
  .label {
    display: flex;
    justify-content: space-between;
    border-bottom: 1px solid #ccc;
    margin-top: 10px;
    padding: 20px 0;
    p {
      font-size: 16px;
    }
    .time {
      font-size: 14px;
      color: #ccc;
    }
  }
  .content-text {
    line-height: 2;
    font-size: 20px;
    padding: 20px 0;
  }
  .content-imglist {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    flex-wrap: wrap;
    padding-left: 6px;
    .imgstyle {
      flex-shrink: 0;
      background: #ccc;
      height: 102px;
      width: 102px;
      margin-right: 6px;
      margin-bottom: 6px;
    }
  }
}
</style>
