<template>
  <div class="homepage">
    <!-- 头部 -->
    <div class="header" id="tabTop">
      <!-- 个人信息 -->
      <div class="user-info">
        <div class="user-name">天空之橙</div>
        <div class="user-desc">星主 陈嵩 已运营1860天</div>
      </div>
    </div>
    <!-- 内容 -->
    <div class="main">
      <div class="main-header">
        <!-- 头像 -->
        <div class="header-image">
          <img
            src="https://thirdwx.qlogo.cn/mmopen/vi_32/DYAIOgq83eobpsN46APV89W3j2eaOJU6QCdfOernzbdChtPJDPBTll8Gl1xWaRKczWUicaQxCSEGOQe1C5rvKHQ/0"
            alt=""
          />
        </div>
        <!-- 个人信息 -->
        <div class="user-info">
          <div class="user-name">
            <div class="user-l">
              陈嵩
              <van-tag type="warning" color="#FFA34B">星主</van-tag>
            </div>
            <div class="user-r" style="color: #0ab18f">3小时前活跃</div>
          </div>
          <div class="user-desc">
            建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计建筑·空间·设计
          </div>
        </div>
      </div>
      <div class="main-content">
        <div class="block">
          <div class="block-header">
            <p>关于我们</p>
            <span><i class="iconfont icon-qizhi-"></i> 投诉</span>
          </div>
          <div class="block-content">
            哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或哈哈说或或或
          </div>
          <van-divider :style="{ color: '#d2d2d2', borderColor: '#d2d2d2', padding: '16px 0', margin: '0 8px' }" />
          <div class="block-bottom">
            <ul class="">
              <li>
                <p style="color: #0ab18f">3小时前</p>
                <p class="tag">最近更新</p>
              </li>
              <li>
                <p>960+</p>
                <p class="tag">成员数</p>
              </li>
              <li>
                <p>960+</p>
                <p class="tag">主题数</p>
              </li>
            </ul>
          </div>
        </div>
      </div>
      <div class="button-bottom">
        <van-button color="#FE7A53" block @click="onclick"> 合作咨询 </van-button>
        <van-button color="#767676" style="width: 80px" @click="goHome">返回 </van-button>
      </div>
    </div>

    <!-- t弹窗 -->
    <van-popup v-model="isshow">
      <div class="popupSrtyle">
        <div
          class="close"
          @click="
            () => {
              isshow = false
            }
          "
        >
          <van-icon name="cross" size="14" />
        </div>
        <p>如需合作！请添加星主好友</p>
        <p class="mini">长按识别二维码</p>
        <div class="codestyle"></div>
        <p>如需合作！请添加星主好友</p>
        <p>如需合作！请添加星主好友</p>
      </div>
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
      isshow: false,
      is_dk: false,
      titleinactivecolor: '#ccc',
      titleactivecolor: '#333',
      linewidth: '20px',
      list: [],
      iszan: false,
      isshareShow: false,
      loading: false,
      finished: false,
      listQuery: {
        label: '',
        page: 1
      },
      zanlist: [],
      typelist: [
        {
          id: '',
          name: '全部'
        },
        {
          id: 1,
          name: '精华'
        },
        {
          id: 2,
          name: '建筑'
        },
        {
          id: 3,
          name: '景观'
        },
        {
          id: 4,
          name: '空间'
        }
        // {
        //   id: 6,
        //   name: ''
        // }
      ]
    }
  },
  created() {
    this.getList()
  },
  methods: {
    goHome() {
      this.$router.replace({ path: '/' })
    },
    onclick() {
      this.isshow = true
    },
    scrollIntoView(id) {
      console.log(this.$el.querySelector(id))
      this.$el.querySelector(id).scrollIntoView({
        behavior: 'smooth', // 平滑过渡
        block: 'start' // 上边框与视窗顶部平齐。默认值
      })
    },
    stopClick(e) {
      console.log(e)
    },
    zanClick(id) {
      if (this.zanlist.includes(id)) {
        this.zanlist = this.zanlist.filter(item => item != id)
      } else {
        this.zanlist.push(id)
      }
    },
    tabchange(name, title) {
      this.listQuery.page = 1
      this.finished = false
      this.scrollIntoView('#tabTop')
      this.getList()
    },
    onDk() {
      if (!this.is_dk) {
        this.is_dk = true
        this.$toast.success('打卡成功')
      }
    },
    onshare() {
      this.isshareShow = true
    },
    getList() {
      this.loading = true
      getPyq(this.listQuery).then(res => {
        res.data = res.data.map(item => {
          item['create_time'] = formatTime(+new Date(item['create_time'].replaceAll('-', '/')))
          return item
        })

        if (this.listQuery.page == 1) {
          this.list = res.data
        } else {
          this.list = this.list.concat(res.data)
        }
        // 加载状态结束
        this.loading = false
        this.listQuery.page++
        // 数据全部加载完成
        if (res.data.length <= 0) {
          this.finished = true
        }
      })
    },
    goDetail(data) {
      this.$router.replace({
        path: '/detail',
        query: {
          id: data.id
        }
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.colorRed {
  color: #fc5531;
  font-size: 24px;
}
.homepage {
  position: relative;
  background: rgba(0, 0, 0, 0);
  overflow: hidden;
  z-index: 1;
  &::before {
    content: '';
    position: absolute;
    top: 0;
    width: 100%;
    height: 320px;
    background: #ccc url('../../assets/bg.jpg') no-repeat;
    background-size: 100% cover;
    z-index: -1;
    animation-name: fadenum12;
    animation-timing-function: ease-in-out;
    animation-iteration-count: infinite;
    animation-duration: 50s;
  }
  //   background-size: cover;
  .header {
    background: rgba(0, 0, 0, 0);
    height: 210px;
    display: flex;
    justify-content: flex-start;
    align-items: flex-end;
    box-sizing: border-box;
    padding-right: 10px;
    .user-info {
      margin-left: 20px;
      color: #fff;
      padding: 20px 0;
      .user-name {
        font-size: 20px;
        line-height: 2;
        font-weight: 700;
      }
      .user-desc {
        font-size: 14px;
        @include textoverflow(2);
      }
    }
  }

  .main {
    background: #eee;
    .main-header {
      display: flex;
      justify-content: flex-start;
      align-items: flex-start;
      box-sizing: border-box;
      padding: 16px 0;
      background: #fff;
      min-height: 80px;
      .header-image {
        width: 38px;
        height: 38px;
        border-radius: 50%;
        overflow: hidden;
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
        margin: 0 15px;
        flex: 1;
        .user-name {
          width: 100%;
          box-sizing: border-box;
          font-size: 14px;
          display: flex;
          justify-content: space-between;
          .user-l {
            color: #441444;
            font-weight: 600;
          }
          .user-r {
            font-size: 12px;
            color: #441444;
          }
        }
        .user-desc {
          font-size: 12px;
          @include textoverflow(2);
        }
      }
    }
    .main-content {
      background: #fff;
      .block {
        margin-top: 14px;
        box-sizing: border-box;
        padding: 14px 20px;

        .block-header {
          display: flex;
          justify-content: space-between;
          align-items: flex-end;
          p {
            font-size: 16px;
            font-weight: 600;
          }
        }
        .block-content {
          padding: 10px 0;
          font-size: 14px;
        }
        .block-bottom {
          ul {
            display: flex;
            justify-content: space-around;
            align-items: center;
            li {
              width: 30%;
              text-align: center;
              p {
                font-weight: 700;
                font-size: 16px;
              }
              .tag {
                font-size: 12px;
                color: #ccc;
              }
            }
          }
        }
      }
    }
    .button-bottom {
      position: fixed;
      background: #fff;
      bottom: 0;
      width: 100%;
      padding: 18px;
      box-sizing: border-box;
      display: flex;
    }
  }
  .van-popup {
    border-radius: 8px;
    overflow: initial;
  }
  .popupSrtyle {
    width: 280px;
    min-height: 280px;
    box-sizing: border-box;
    padding: 10px;
    text-align: center;
    border-radius: 8px;
    font-size: 14px;
    .mini {
      color: #ccc;
      font-size: 12px;
    }
    .codestyle {
      background: #ccc;
      height: 120px;
      width: 120px;
      margin: 20px auto;
      border-bottom: 1px solid #ccc;
    }
    .close {
      position: absolute;
      top: -10px;
      right: -10px;
      width: 20px;
      height: 20px;
      background: #333;
      text-align: center;
      line-height: 20px;
      border-radius: 50%;
      color: #fff;
      padding: 4px;
    }
  }
}
</style>
