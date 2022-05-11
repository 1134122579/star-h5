<template>
  <div class="homepage">
    <!-- 头部 -->
    <div class="header">
      <!-- 头像 -->
      <div class="header-image">
        <img
          src="https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83eobpsN46APV89W3j2eaOJU6QCdfOernzbdChtPJDPBTll8Gl1xWaRKczWUicaQxCSEGOQe1C5rvKHQ/0"
          alt=""
        />
      </div>
      <!-- 个人信息 -->
      <div class="user-info">
        <div class="user-name">陈嵩 | 盛视天橙™天空之橙</div>
        <div class="user-desc">建筑·空间·设计</div>
      </div>
    </div>
    <!-- 内容 -->
    <div class="main">
      <div class="main-header">
        <ul class="flexbetween">
          <li @click="onDk">
            <i class="iconfont icon-sign icon-size"></i>
            <p>打卡</p>
          </li>
          <li>
            <i class="iconfont icon-dingyue icon-size"></i>
            <p>订阅</p>
          </li>
          <li>
            <i class="iconfont icon-setting icon-size"></i>
            <p>设置</p>
          </li>
          <li @click="onshare">
            <i class="iconfont icon-fenxiang icon-size"></i>
            <p>分享</p>
          </li>
        </ul>
      </div>
      <div class="main-content">
        <van-sticky>
          <div class="main-content-tab">
            <van-tabs
              v-model="listQuery.label"
              :title-inactive-color="titleinactivecolor"
              :title-active-color="titleactivecolor"
              :color="titleactivecolor"
              :line-width="linewidth"
              @change="tabchange"
            >
              <van-tab v-for="(item, index) in typelist" :name="item.id" :title="item.name"> </van-tab>
            </van-tabs>
          </div>
        </van-sticky>
        <!-- 列表 -->
        <div class="man-list">
          <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="getList">
            <div class="block-style" @click="goDetail(item)" v-for="(item, index) in list">
              <div class="user-headerimage">
                <i class="iconfont icon-huangguan huangguanstyle"></i>
                <img :src="item.headimgurl" alt="" />
              </div>
              <div class="block-content">
                <div class="block-user-info">
                  <div class="block-name">{{ item.nickname }}</div>
                  <div class="block-time">{{ item.create_time }}</div>
                </div>
                <!-- 文本内容 -->
                <div class="block-text">
                  {{ item.title }}
                </div>
                <!-- 图片 -->
                <div class="block-list">
                  <div class="imgonesrtle" v-for="(imgItem, index) in item.imgs"><img :src="imgItem" /></div>
                </div>
                <!-- 操作 -->
                <div class="block-button">
                  <i class="iconfont icon-zan"></i>
                  <i class="iconfont icon-duihuaqipao"></i>
                </div>
              </div>
            </div>
          </van-list>
        </div>
      </div>
    </div>
    <!-- 分享提示 -->
    <van-popup
      v-model="isshareShow"
      :close-on-popstate="true"
      position="top"
      :style="{ maxHeight: '100%', background: 'rgba(0,0,0,0)' }"
    >
      <img
        style="width: 100%; box-sizing: border-box; padding: 20px 20px 56px 144px"
        src="@/assets/fx.png"
        alt=""
        @click="
          () => {
            isshareShow = false
          }
        "
      />
    </van-popup>
  </div>
</template>

<script>
import { getPyq } from '@/api/user'
import { formatTime } from '@/utils'
import { setShareInfo } from '@/utils/share'

export default {
  data() {
    return {
      titleinactivecolor: '#ccc',
      titleactivecolor: '#333',
      linewidth: '20px',
      list: [],
      isshareShow: false,
      loading: false,
      finished: false,
      listQuery: {
        type: 1,
        label: 1,
        page: 1
      },
      typelist: [
        {
          id: 1,
          name: '全部'
        },
        {
          id: 2,
          name: '精华'
        },
        {
          id: 3,
          name: '建筑'
        },
        {
          id: 4,
          name: '景观'
        },
        {
          id: 5,
          name: '空间'
        },
        {
          id: 6,
          name: '打卡'
        }
      ]
    }
  },
  created() {
    // this.getList()
    let wxConfig = {
      title: '天空之橙·Design｜建筑·空间·景观·运营',
      url: location.href,
      desc: '',
      link: window.location.origin + window.location.pathname,
      imgUrl: 'http://api.skyorange.cn/logo.jpg'
    }
    setShareInfo(wxConfig)
  },
  methods: {
    tabchange(name, title) {
      console.log(name, title, this.listQuery)
      this.listQuery.page = 1
      this.list = []
      this.finished = false
      //   this.getList()
    },
    onDk() {
      this.$toast.success('打卡成功')
    },
    onshare() {
      this.isshareShow = true
    },
    getList() {
      getPyq(this.listQuery).then(res => {
        res.data = res.data.map(item => {
          item['create_time'] = formatTime(+new Date(item['create_time']))
          return item
        })
        this.list = this.list.concat(...res.data)
        // 加载状态结束
        this.loading = false
        this.listQuery.page = this.listQuery.page + 1
        // 数据全部加载完成
        if (res.data.length <= 0) {
          this.finished = true
        }
      })
    },
    goDetail(data) {
      this.$router.push({
        path: '/detail',
        query: {
          id: data.id
        }
      })
    }
  }
}
</script>

<style lang="scss">
.homepage {
  background: #ccc url('../../assets/bg.jpg') no-repeat;
  //   background-size: cover;
  .header {
    height: 200px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    box-sizing: border-box;
    padding-right: 10px;
    .header-image {
      width: 74px;
      height: 74px;
      border-radius: 50%;
      overflow: hidden;
      background: #ccc;
      margin-left: 25px;
      flex-shrink: 0;
      img {
        width: 100%;
        height: 100%;
        display: block;
        object-fit: cover;
      }
    }
    .user-info {
      margin-left: 15px;
      color: #fff;
      .user-name {
        font-size: 18px;
      }
      .user-desc {
        font-size: 14px;
        @include textoverflow(2);
      }
    }
  }

  .main {
    .main-header {
      padding: 10px 0;
      background: rgba(0, 0, 0, 0.4);
      color: #fff;
      border-radius: 10px 10px 0 0;
      ul {
        justify-content: space-around;
        font-size: 12px;
        li {
          text-align: center;
          .icon-size {
            font-size: 20px;
          }
          p {
            text-align: center;
          }
        }
      }
    }
    .main-content {
      background: #fff;
      .man-list {
        padding: 10px 20px;
        .block-style {
          padding: 14px 0;
          width: 100%;
          display: flex;
          .user-headerimage {
            width: 40px;
            height: 40px;
            flex-shrink: 0;
            border-radius: 50%;
            background: #ccc;
            position: relative;
            margin-right: 10px;
            img {
              width: 100%;
              height: 100%;
              object-fit: cover;
              display: block;
              border-radius: 50%;
            }
            .huangguanstyle {
              font-size: 20px;
              flex: 1;
              color: burlywood;
              position: absolute;
              top: -20px;
              left: 0;
              right: 0;
              margin: 0 auto;
              text-align: center;
            }
          }
          .block-content {
            flex: 1;
            .block-user-info {
              font-size: 14px;
            }
            .block-time {
              font-size: 10px;
              color: #ccc;
            }
            .block-text {
              font-size: 16px;
              margin-top: 6px;
              @include textoverflow(5);
            }
            .block-list {
              display: flex;
              justify-content: flex-start;
              align-items: flex-start;
              flex-wrap: wrap;
              .imgonesrtle {
                margin-top: 4px;
                margin-right: 4px;
                flex-shrink: 0;
                width: 90px;
                height: 90px;
                background: #ccc;
                img {
                  width: 100%;
                  height: 100%;
                  object-fit: cover;
                  display: block;
                }
              }
            }
            .block-button {
              margin-top: 10px;
              i {
                font-size: 20px;
                margin-right: 34px;
              }
            }
          }
        }
      }
    }
  }
}
</style>
