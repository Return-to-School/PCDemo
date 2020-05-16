<template>
  <div>
    <el-menu
      :default-active="indexSwitch"
      class="el-menu-demo"
      mode="horizontal"
      @select="handleRedirect"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b">
      <el-menu-item index="1">
        <i class="el-icon-odometer"></i>
        仪表盘
      </el-menu-item>
      <el-menu-item index="4">账号管理</el-menu-item>
    </el-menu>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="14">
        <div class="grid-content">
          <h3 class="title f">

          </h3>
          <div class="subtitle">
            请选择一个功能进入。
          </div>
        </div>
      </el-col>
    </el-row>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="14">
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span><strong>功能</strong></span>
          </div>
          <el-menu @select="handleRedirect">
            <el-menu-item index="1">
              <i class="el-icon-menu"></i>
              <span slot="title">活动列表</span>
            </el-menu-item>
            <el-menu-item index="2">
              <i class="el-icon-menu"></i>
              <span slot="title">新增活动</span>
            </el-menu-item>
            <el-menu-item index="3">
              <i class="el-icon-menu"></i>
              <span slot="title">人员导出</span>
            </el-menu-item>
          </el-menu>
        </el-card>
      </el-col>
    </el-row>
    <div class="box"></div>
    <el-row type="flex" class="row-bg" justify="center" :gutter="20">
      <el-col :span="14">
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span><strong>通知</strong></span>
          </div>
          <el-alert type="success" title="暂时没有新内容。"></el-alert>
          <el-divider></el-divider>
        </el-card>
      </el-col>
    </el-row>
    <div class="box"></div>
    <el-row type="flex" class="row-bg" justify="center" :gutter="20">
      <el-col :span="14">
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span><strong>近日安排</strong></span>
          </div>
        </el-card>
      </el-col>
    </el-row>
    <el-divider></el-divider>
    <el-button v-trigger @click="autoClick">
    </el-button>
  </div>
</template>

<script>
  export default {
    methods: {
      handleClick(row) {
        console.log(row);
      },
      handleRedirect(index, path) {
        if (index == 1) this.$router.push({path: '/list?url=dashboard'});
        else if (index == 2) this.$router.push({path: '/new?url=dashboard&action=new&sign='});
        else if (index == 4) this.$router.push({path: '/manage?url=dashboard'});
        else this.$router.push({path: '/print'});
      },
      autoClick() {
        let timeNow = new Date();
        let hours = timeNow.getHours();
        let text = '';
        if (hours >= 0 && hours <= 10) {
          text = '早上好，';
        } else if (hours > 10 && hours <= 14) {
          text = '中午好，';
        } else if (hours > 14 && hours <= 18) {
          text = '下午好，';
        } else if (hours > 18 && hours <= 24) {
          text = '晚上好，';
        }
        $('.f').text(text);
      }
    },
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'}
        ],
        style: [],
        title: '仪表盘'
      }
    },
    directives: {
      trigger: {
        inserted(el, b) {
          el.click();
        }
      }
    },
    data() {
      return {
        indexSwitch: '1'
      };
    },
  }
</script>

<style scoped>
  .grid-content {
    border-radius: 4px;
    margin: 0.5cm 0 0.5cm 0;
    min-height: 36px;
  }

  .title {
    font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
    display: block;
    font-weight: 300;
    font-size: 60px;
    color: #35495e;
    letter-spacing: 1px;
  }

  .subtitle {
    font-weight: 300;
    font-size: 42px;
    color: #526488;
    word-spacing: 5px;
    padding-bottom: 15px;
  }

  .box {
    height: 10px;
  }
</style>
