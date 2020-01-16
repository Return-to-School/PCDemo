<template>
  <div>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="18">
        <div class="grid-content">
          <el-button type="primary" round @click="goBack()">返回</el-button>
          <el-divider direction="vertical"></el-divider>
          <span>活动详情</span>
        </div>
        <el-divider></el-divider>
      </el-col>
    </el-row>
    <el-row type="flex" class="row-bg" justify="center" :gutter="20">
      <el-col :span="4">
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span>活动状态</span>
            <el-tag style="float:right" type="danger">关闭</el-tag>
          </div>
          <div class="item">
            <el-timeline :reverse="false">
              <el-timeline-item v-for="(activity,index) in activities" :key="index" :timestamp="activity.time">
                {{ activity.content }}
              </el-timeline-item>
            </el-timeline>
          </div>
        </el-card>
      </el-col>
      <el-col :span="14">
        <div style="display: inline">
          <div class="subtitle">
            <i class="el-icon-more"></i>
            活动详情
            <div style="float:right">
              <el-button-group>
                <el-button type="primary" @click="changeActivity()">修改活动</el-button>
                <el-button type="primary" @click="apply=true">人员审核</el-button>
                <el-button type="primary" @click="complete=true">反馈考评</el-button>
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
                活动名称：
                <el-tag type="">一个神奇的活动</el-tag>
              </div>
            </el-col>
            <el-col :span="8">
              <div class="grid-content">
                上限人数：
                <el-tag type="info">200 人</el-tag>
              </div>
            </el-col>
          </el-row>
          <el-row type="flex" class="row-bg" justify="space-around">
            <el-col :span="8">
              <div class="grid-content">
                开始时间：
                <el-tag type="success">2020-1-10</el-tag>
              </div>
            </el-col>
            <el-col :span="8">
              <div class="grid-content">
                结束时间：
                <el-tag type="danger">2020-1-11</el-tag>
              </div>
            </el-col>
          </el-row>
        </el-card>
        <div class="box"></div>
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span><strong>活动描述</strong></span>
          </div>
          <p>这里全部都是描述内容</p>
        </el-card>
        <div class="box"></div>
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span><strong>详细信息</strong></span>
          </div>
        </el-card>
        <div class="box"></div>
        <el-card shadow="hover">
          <div slot="header" class="clearfix">
            <span><strong>更多信息</strong></span>
          </div>
        </el-card>
        <el-dialog title="人员审核" :visible.sync="apply">
          <el-card shadow="hover">
            <div slot="header" class="clearfix">
              <span><strong>处理情况</strong></span>
            </div>
            <el-row type="flex" class="row-bg" justify="space-between">
              <el-col :span="6">
                <div class="grid-content">
                  总报名人数：
                  <el-tag type="info">200</el-tag>
                </div>
              </el-col>
              <el-col :span="6">
                <div class="grid-content">
                  通过人数：
                  <el-tag type="success">100</el-tag>
                </div>
              </el-col>
              <el-col :span="6">
                <div class="grid-content">
                  未处理人数：
                  <el-tag type="warning">100</el-tag>
                </div>
              </el-col>
            </el-row>
          </el-card>
          <div class="box"></div>
          <el-table border :data="applyData" style="width:100%" max-height="400">
            <el-table-column fixed prop="id" label="申请编号">
            </el-table-column>
            <el-table-column prop="name" label="姓名">
            </el-table-column>
            <el-table-column prop="major" label="专业">
            </el-table-column>
            <el-table-column prop="class" label="班级">
            </el-table-column>
            <el-table-column prop="no" label="学号">
            </el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <div v-if="scope.row.status==-1">
                  <el-button @click="handleClick(scope.row)" type="text" size="small">通过</el-button>
                  <el-button @click="handleClick(scope.row)" type="text" size="small">拒绝</el-button>
                </div>
                <div v-else-if="scope.row.status==0">
                  <el-tag type="danger">已拒绝</el-tag>
                </div>
                <div v-else>
                  <el-tag type="success">已通过</el-tag>
                </div>
              </template>
            </el-table-column>
          </el-table>
        </el-dialog>
        <el-dialog title="反馈考评" :visible.sync="complete">
          <el-table border :data="applyData" style="width:100%" max-height="400">
            <el-table-column fixed prop="id" label="申请编号">
            </el-table-column>
            <el-table-column prop="name" label="姓名">
            </el-table-column>
            <el-table-column prop="major" label="专业">
            </el-table-column>
            <el-table-column prop="class" label="班级">
            </el-table-column>
            <el-table-column prop="no" label="学号">
            </el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-dialog>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    methods: {
      handleClick(row) {
        console.log(row);
      },
      goBack() {
        this.$router.push({path: '/list'});
      },
      changeActivity() {
        this.$router.push({path: '/new?url=describe'});
      }
    },
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'}
        ],
        style: [],
        title: '活动详情'
      }
    },
    data() {
      return {
        activities: [{
          content: '创建活动',
          time: '2020-1-10'
        },
          {
            content: '关闭活动',
            time: '2020-1-11'
          }],
        apply: false,
        complete: false,
        applyData: [{
          id: '#00001',
          name: '张三',
          major: '电子信息专业',
          class: '191班',
          no: '6105119151',
          status: -1/* 关于状态的定义：-1是没有操作，1是已经通过，0是已经拒绝 */
        },
          {
            id: '#00002',
            name: '李四',
            major: '电子信息专业',
            class: '191班',
            no: '6105119151',
            status: 1
          },
          {
            id: '#00003',
            name: '王五',
            major: '电子信息专业',
            class: '191班',
            no: '6105119151',
            status: 0
          }],
        completeData: [{
          id: '#00001',
          name: '张三',
          major: '电子信息专业',
          class: '191班',
          no: '6105119151',
        },
          {
            id: '#00002',
            name: '李四',
            major: '电子信息专业',
            class: '191班',
            no: '6105119151',
          },
          {
            id: '#00003',
            name: '王五',
            major: '电子信息专业',
            class: '191班',
            no: '6105119151',
          }]
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
