<template>
  <div>
    <el-menu
      :default-active="indexSwitch"
      class="el-menu-demo"
      mode="horizontal"
      @select="handleSelect"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b">
      <el-menu-item index="1">
        <i class="el-icon-odometer"></i>
        仪表盘
      </el-menu-item>
      <el-menu-item index="2">账号管理</el-menu-item>
    </el-menu>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="14">
        <div class="grid-content">
          <h3 class="title">
            上午好，
          </h3>
          <div class="subtitle">
            您可以由本页面进入所有功能。
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
          <el-alert type="error" title="这是一条重要通知"></el-alert>
          <el-divider></el-divider>
          <el-alert type="error" title="这是一条重要通知"></el-alert>
          <el-divider></el-divider>
          <el-alert type="error" title="这是一条重要通知"></el-alert>
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
  </div>
</template>

<script>
  export default {
    methods: {
      handleClick(row) {
        console.log(row);
      },
      handleRedirect(index,path){
        if(index==1) this.$router.push({path: '/list?url=dashboard'});
        else if(index==2) this.$router.push({path: '/new?url=dashboard'});
        else this.$router.push({path: '/print'});
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
    data() {
      return {
        indexSwitch:'1'
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
