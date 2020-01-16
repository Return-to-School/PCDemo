<template>
  <div>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="18">
        <div class="grid-content">
          <el-button type="primary" round @click="goBack()">返回</el-button>
          <el-divider direction="vertical"></el-divider>
          <span>学生资料</span>
        </div>
        <el-divider></el-divider>
      </el-col>
    </el-row>
    <el-row type="flex" class="row-bg" justify="center" :gutter="20">
      <el-col :span="6">
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span>请先阅读</span>
          </div>
          <el-collapse accordion>
            <el-collapse-item title="关于评分" name="1">
              <el-alert type="error" title="请注意：您在本页面对该学生的评分可能会影响其综合评分" :closable="false"></el-alert>
              <div>评分方式采用星级制。请您点击右上角"考评"按钮，并在弹出的对话框中使用鼠标点击星星进行评分。<strong>当前活动对该学生的评分能够且仅能够进行一次。</strong></div>
            </el-collapse-item>
            <el-collapse-item title="其他信息" name="2">
              <div>等待更新。</div>
            </el-collapse-item>
          </el-collapse>
        </el-card>
      </el-col>
      <el-col :span="12">
        <div style="display: inline">
          <div class="subtitle">
            <i class="el-icon-user-solid"></i>
            学生资料
            <div style="float:right">
              <el-button-group>
                <el-button type="primary">下载学生反馈材料</el-button>
                <el-button type="primary" @click="rate=true">考评</el-button>
              </el-button-group>
            </div>
          </div>
        </div>
        <div class="box"></div>
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span><strong>基本信息</strong></span>
          </div>
          <el-row type="flex" class="row-bg" justify="space-around">
            <el-col :span="8">
              <div class="grid-content">
                姓名：
                <el-tag type="">张三</el-tag>
              </div>
            </el-col>
            <el-col :span="8">
              <div class="grid-content">
                专业：
                <el-tag type="info">电子信息类</el-tag>
              </div>
            </el-col>
          </el-row>
          <el-row type="flex" class="row-bg" justify="space-around">
            <el-col :span="8">
              <div class="grid-content">
                班级：
                <el-tag type="success">191</el-tag>
              </div>
            </el-col>
            <el-col :span="8">
              <div class="grid-content">
                学号：
                <el-tag type="danger">1234567890</el-tag>
              </div>
            </el-col>
          </el-row>
        </el-card>
        <div class="box"></div>
        <el-card shadow="hover">

        </el-card>
      </el-col>
    </el-row>
    <el-dialog title="考评" :visible.sync="rate" width="30%" center>
      <el-rate v-modal="rate_v" show-text style="text-align: center" @change="rateHandle($event)"></el-rate>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    methods: {
      handleClick(row) {
        console.log(row);
      },
      goBack() {
        this.$router.push({path: '/describe'});
      },
      rateHandle(score){
        console.log(score);
      }
    },
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'}
        ],
        style: [],
        title: '学生资料'
      }
    },
    data() {
      return{
        rate:false,
        rate_v:null,
        texts:['极差','较差','一般','较好','极好']
      }
    }
  }
</script>

<style scoped>
  .grid-content {
    border-radius: 4px;
    margin: 0.5cm 0 0.5cm 0;
    min-height: 36px;
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
