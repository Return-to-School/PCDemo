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
            <el-tag style="float:right" type="info">正常</el-tag>
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
        <div class="content">
          <div style="display: inline">
            <div class="subtitle">
              <i class="el-icon-more"></i>
              活动详情
              <div style="float:right">
                <el-button-group>
                  <el-button type="primary" @click="updateActivity()">修改活动</el-button>
                  <el-button type="primary" @click="openStudentModal(1)">学生信息</el-button>
                  <el-button type="primary" @click="openApplyModal(1)">人员审核</el-button>
                  <el-button type="primary" @click="openCompleteModal(1)">反馈考评</el-button>
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
                  <el-tag type="">{{name}}</el-tag>
                </div>
              </el-col>
              <el-col :span="8">
                <div class="grid-content">
                  创建用户：
                  <el-tag type="info">{{creator}}</el-tag>
                </div>
              </el-col>
            </el-row>
            <el-row type="flex" class="row-bg" justify="space-around">
              <el-col :span="8">
                <div class="grid-content">
                  申请开始时间：
                  <el-tag type="success">{{applyStartTime}}</el-tag>
                </div>
              </el-col>
              <el-col :span="8">
                <div class="grid-content">
                  申请结束时间：
                  <el-tag type="success">{{applyEndTime}}</el-tag>
                </div>
              </el-col>
            </el-row>
            <el-row type="flex" class="row-bg" justify="space-around">
              <el-col :span="8">
                <div class="grid-content">
                  考评开始时间：
                  <el-tag type="success">{{applyStartTime}}</el-tag>
                </div>
              </el-col>
              <el-col :span="8">
                <div class="grid-content">
                  考评结束时间：
                  <el-tag type="success">{{applyEndTime}}</el-tag>
                </div>
              </el-col>
            </el-row>
            <el-row type="flex" class="row-bg" justify="space-around">
              <el-col :span="8">
                <div class="grid-content">
                  活动创建时间：
                  <el-tag type="success">{{createTime}}</el-tag>
                </div>
              </el-col>
              <el-col :span="8">
                <div class="grid-content">
                  活动区域设置：
                  <el-tag type="success">{{location}}</el-tag>
                </div>
              </el-col>
            </el-row>
          </el-card>
          <div class="box"></div>
          <el-card shadow="hover">
            <div slot="header" class="clearfix">
              <span><strong>活动描述</strong></span>
            </div>
            <p>{{describe}}</p>
          </el-card>
          <div class="box"></div>
          <el-dialog title="人员审核" :visible.sync="apply">
            <el-card shadow="hover">
              <div slot="header" class="clearfix">
                <span><strong>处理情况</strong></span>
              </div>
              <el-row type="flex" class="row-bg" justify="space-between">
                <el-col :span="6">
                  <div class="grid-content">
                    总报名人数：
                    <el-tag type="info">未配置</el-tag>
                  </div>
                </el-col>
                <el-col :span="6">
                  <div class="grid-content">
                    通过人数：
                    <el-tag type="success">未配置</el-tag>
                  </div>
                </el-col>
                <el-col :span="6">
                  <div class="grid-content">
                    未处理人数：
                    <el-tag type="warning">未配置</el-tag>
                  </div>
                </el-col>
              </el-row>
            </el-card>
            <div class="box"></div>
            <el-table border :data="applyData" style="width:100%" max-height="400" @selection-change="changeSelect">
              <el-table-column type="selection" width="55" :selectable="selectable">
              </el-table-column>
              <el-table-column prop="student.name" label="姓名">
              </el-table-column>
              <el-table-column prop="student.gender.desc" label="性别">
              </el-table-column>
              <el-table-column prop="studentId" label="学号">
              </el-table-column>
              <el-table-column prop="student.highSchool" label="回访高中">
              </el-table-column>
              <el-table-column prop="student.college" label="专业">
              </el-table-column>
              <el-table-column label="状态">
                <template slot-scope="scope">
                  <div v-if="scope.row.status.code==0">
                    <el-tag type="info">未处理</el-tag>
                  </div>
                  <div v-else-if="scope.row.status.code==2">
                    <el-tag type="danger">已拒绝</el-tag>
                  </div>
                  <div v-else>
                    <el-tag type="success">已通过</el-tag>
                  </div>
                </template>
              </el-table-column>
            </el-table>
            <div style="margin-top:20px">
              <el-pagination background layout="total,prev, pager, next" :total="applyTableLength"
                             @current-change="openApplyModal">
              </el-pagination>
            </div>
            <div slot="footer" class="dialog-footer">
              <el-button type="success" @click="handleSelection(1)">批量通过</el-button>
              <el-button type="danger" @click="handleSelection(2)">批量拒绝</el-button>
            </div>
          </el-dialog>
          <el-dialog title="反馈考评" :visible.sync="complete">
            <el-table border :data="feedbackData" style="width:100%" max-height="400">
              <el-table-column prop="student.name" label="姓名">
              </el-table-column>
              <el-table-column prop="student.gender.desc" label="性别">
              </el-table-column>
              <el-table-column prop="studentId" label="学号">
              </el-table-column>
              <el-table-column prop="student.highSchool" label="回访高中">
              </el-table-column>
              <el-table-column prop="student.college" label="专业">
              </el-table-column>
              <el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div style="margin-top:20px">
              <el-pagination background layout="total,prev, pager, next" :total="feedbackTableLength"
                             @current-change="openCompleteModal">
              </el-pagination>
            </div>
          </el-dialog>
          <el-dialog title="学生信息" :visible.sync="students">
            <el-table border :data="studentsData" style="width:100%" max-height="400">
              <el-table-column prop="student.name" label="姓名">
              </el-table-column>
              <el-table-column prop="student.studentId" label="学号">
              </el-table-column>
              <el-table-column prop="student.college" label="专业">
              </el-table-column>
              <el-table-column prop="student.origin" label="籍贯">
              </el-table-column>
              <el-table-column prop="student.highSchool" label="回访高中">
              </el-table-column>
              <el-table-column prop="description" label="备注">
              </el-table-column>
            </el-table>
            <div style="margin-top:20px">
              <el-pagination background layout="total,prev, pager, next" :total="studentTableLength"
                             @current-change="openStudentModal">
              </el-pagination>
            </div>
            <div slot="footer" class="dialog-footer">
              <el-button type="primary" @click="expStudents()">导出所有学生信息</el-button>
            </div>
          </el-dialog>
        </div>

      </el-col>
    </el-row>
    <el-button v-trigger @click="autoClick">
    </el-button>
  </div>
</template>

<script>
  import axios from 'axios';

  const myaxios = axios.create({
    baseURL: "http://101.37.173.57:8080/",
    timeout: 5000,
    headers: {
      //传入头数据
    }
  });

  export default {
    methods: {
      autoClick() {
        //进入页面自动执行
        const id = this.$route.query.id;//查询url嵌入的list对象id
        const store = window.sessionStorage;
        if (store.getItem('list_' + id) == null) {
          $('.content').text();//将页面内容设置为空，截停查询操作
          alert("很抱歉，您的提交参数有误，请重新操作进入本页面！");
          this.$router.push({path: '/list?url=dashboard'}); //直接将页面退出
        } else {
          const lstData = JSON.parse(store.getItem('list_' + id));
          this.name = lstData.name;
          this.describe = lstData.content;
          this.applyStartTime = lstData.applyStartTime;
          this.applyEndTime = lstData.applyEndTime;
          this.feedbackStartTime = lstData.feedbackStartTime;
          this.feedbackEndTime = lstData.feedbackEndTime;
          this.creator = lstData.creator;
          this.location = lstData.location;
          this.createTime = lstData.createTime;
          this.activities = [{
            content: '创建活动',
            time: lstData.createTime,
          }];
        }
      },
      handleClick(row) {
        //跳转学生详情页面
        const store = window.sessionStorage;//设置session缓存
        store.setItem('feedback_' + row.applyId, JSON.stringify(row));//将用户指定的list对象放入session并保持最新
        this.$router.push({path: '/rate?id=' + row.applyId});
        console.log(row);
      },
      goBack() {
        this.$router.push({path: '/list?url=dashboard'});
      },
      updateActivity() {
        const store = window.sessionStorage;//设置session缓存
        const id = this.$route.query.id;//查询url嵌入的list对象id
        if (store.getItem('update_' + id) == null) {
          store.setItem('update_' + id, store.getItem('list_' + id));//将内容存入list
        }
        this.$router.push({path: '/new?url=describe&action=update&sign=' + id});
      },
      selectable(row, index) {
        //控制复选框可选状态（可选函数）
        /*if (row.status == -1) {
          return true;
        } else {
          return false;
        }*/
        return true;
      },
      openApplyModal(pages) {
        //先进行数据注入，再显示对话框
        //复用方法，根据pages的不同完成数据初始化、分页加载
        //this.apply = true;//加载对话框
        const id = this.$route.query.id;//查询url嵌入的list对象id
        myaxios.get('/student/student-in-act/' + id + '?currPage=' + pages + '&pageSize=10').then((res) => {
          console.log(res.data);
          this.applyTableLength = res.data.data.totalCount;//加载数据总条数，重复加载以应对并发操作
          this.applyData = res.data.data.result;//注入表格数据
          this.apply = true;//加载对话框
        }).catch((e) => {
          //加载错误中断请求
          this.$message.error('请求数据失败！请刷新页面重试');
          this.applyData = '';
        });
      },
      openStudentModal(pages) {
        //this.students = true;//加载对话框
        const store = window.sessionStorage;
        const userid = store.getItem('user');
        const id = this.$route.query.id;//查询url嵌入的list对象id
        myaxios.post('/apply/search-for-group/' + userid + '?currPage=' + pages + '&pageSize=10', {
          activityId: id
        }).then((res) => {
          this.studentTableLength = res.data.totalCount;//加载数据总条数，重复加载以应对并发操作
          this.studentsData = res.data.result;//注入表格数据
          this.students = true;//加载对话框
        }).catch((e) => {
          //加载错误中断请求
          this.$message.error('请求数据失败！请刷新页面重试');
          this.applyData = '';
        });
      },
      openCompleteModal(pages) {
        const id = this.$route.query.id;//查询url嵌入的list对象id
        myaxios.get('/activity/' + id + '/apply-pass?currPage=' + pages + '&pageSize=10').then((res) => {
          this.feedbackTableLength = res.data.totalCount;//加载数据总条数，重复加载以应对并发操作
          this.feedbackData = res.data.result;//注入表格数据
          this.complete = true;//加载对话框
        }).catch((e) => {
          //加载错误中断请求
          this.$message.error('请求数据失败！请刷新页面重试');
          this.feedbackData = '';
        })
      },
      expStudents() {
        const dataList = this.studentsData;//拿出学生数据
        const store = window.sessionStorage;
        const id = this.$route.query.id;//查询url嵌入的list对象id
        axios({
          method:'post',
          url:'http://101.37.173.57:8080/apply/export-for-group/'+ store.getItem('user'),
          data:{
            activityId: id
          },
          responseType: 'blob'
        }).then((res) => {
          this.$message.success('导出请求提交成功！');
          this.download(res.data);
        }).catch((e) => {
          this.$message.error('操作提交失败！请刷新页面重试');
        });
      },
      changeSelect(s) {
        //每一次更改选项都会重新传入目前完整的选择列表（json array）
        this.selectedData = s;
      },
      handleSelection(status) {
        //处理目前为止的所有多选行
        const dataList = this.selectedData;//反向查询数据
        var i = 0, line;
        var request = [];//声明一个新数组
        for (i = 0; i < dataList.length; i++) {
          line = {applyId: dataList[i].applyId};//提取每一行数据的id
          request.push(line);
        }
        const jsonRequest = JSON.stringify(request);
        axios({
          method: 'post',
          url: 'http://101.37.173.57:8080/apply/examination/' + status,
          data: jsonRequest,
          headers: {'Content-Type': 'application/json'}
        }).then((res) => {
          this.applyData = '';//防止脏数据产生
          this.apply = false;//关掉对话框，重新打开对话框即刷新数据
          this.$message.success('审核操作已提交');
        }).catch((e) => {
          //加载错误中断请求
          this.$message.error('操作提交失败！请刷新页面重试');
        });
      },
      download(data) {
        let url = window.URL.createObjectURL(new Blob([data],{type:"application/vnd.ms-excel;charset=utf-8"}));
        let link = document.createElement('a');
        link.style.display = 'none';
        link.href = url;
        link.setAttribute('download', 'list.xls');
        document.body.appendChild(link);
        link.click();
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
    directives: {
      trigger: {
        inserted(el, b) {
          el.click();
        }
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
        students: false,
        name: '等待处理',
        creator: '等待处理',
        describe: '等待处理',
        applyStartTime: '等待处理',
        applyEndTime: '等待处理',
        location: '等待处理',
        createTime: '等待处理',
        applyTableLength: 0,//先默认有0行数据
        feedbackTableLength: 0,
        studentTableLength: 0,
        selectedData: [],//隐藏函数
        applyData: [],/* 关于状态的定义：-1是没有操作，1是已经通过，0是已经拒绝 */
        feedbackData: [],
        studentsData: []
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
