<template>
  <div>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="18">
        <div class="grid-content">
          <el-button type="primary" round @click="goBack()">返回</el-button>
          <el-divider direction="vertical"></el-divider>
          <span>活动列表</span>
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
            <el-collapse-item title="关于历史活动" name="1">
              <div>历史活动通常是已经关闭的活动，您仅有查看记录的权限，而无法对活动进行任何修改。</div>
            </el-collapse-item>
          </el-collapse>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-tabs type="border-card">
          <el-tab-pane label="所有活动">
            <div class="subtitle">
              <i class="el-icon-s-order"></i>
              所有活动
            </div>
            <el-alert title="请选择一个活动并开始操作" type="info" description="浏览活动列表，并选取您需要进行的操作" show-icon>
            </el-alert>
            <div class="box"></div>
            <el-card shadow="hover">
              <el-table border :data="tableData" style="width:100%" max-height="500" v-loading="loadings">
                <el-table-column fixed prop="activityId" label="活动编号">
                </el-table-column>
                <el-table-column prop="name" label="活动名称">
                </el-table-column>
                <el-table-column prop="applyStartTime" label="活动开始日期">
                </el-table-column>
                <el-table-column prop="applyEndTime" label="活动结束日期">
                </el-table-column>
                <el-table-column prop="location" label="活动区域">
                </el-table-column>
                <el-table-column label="操作">
                  <template slot-scope="scope">
                    <el-button @click="handleClick(scope.row,1)" type="text" size="small">查看</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div style="margin-top:20px">
                <el-pagination background layout="total, prev, pager, next" :total="tableLength"
                               @current-change="listChange">
                </el-pagination>
              </div>
            </el-card>
          </el-tab-pane>
          <el-tab-pane label="历史活动">
            <div class="subtitle">
              <i class="el-icon-s-order"></i>
              历史活动
            </div>
            <el-alert title="请选择一个活动并开始操作" type="info" description="浏览活动列表，并选取您需要进行的操作" show-icon>
            </el-alert>
            <div class="box"></div>
            <el-card shadow="hover">
              <el-table border :data="historyData" style="width:100%" max-height="500" v-loading="loadings">
                <el-table-column fixed prop="activityId" label="活动编号">
                </el-table-column>
                <el-table-column prop="name" label="活动名称">
                </el-table-column>
                <el-table-column prop="applyStartTime" label="活动开始日期">
                </el-table-column>
                <el-table-column prop="applyEndTime" label="活动结束日期">
                </el-table-column>
                <el-table-column prop="location" label="活动区域">
                </el-table-column>
                <el-table-column label="操作">
                  <template slot-scope="scope">
                    <el-button @click="handleClick(scope.row,2)" type="text" size="small">查看</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div style="margin-top:20px">
                <el-pagination background layout="total, prev, pager, next" :total="historyTableLength"
                               @current-change="historyListChange">
                </el-pagination>
              </div>
            </el-card>
          </el-tab-pane>
        </el-tabs>
      </el-col>
    </el-row>
    <el-button v-trigger @click="autoClick" style="visibility: hidden;">
    </el-button>
  </div>
</template>

<script>
  import axios from 'axios'

  const myaxios = axios.create({
    baseURL: "http://101.37.173.57:8080/",
    timeout: 5000,
    withCredentials: true,
    headers: {
      //传入头数据
    }
  });

  export default {
    asyncData() { //进入页面前的预加载
      myaxios.interceptors.request.use(function (config) {
        return config;
      });
      return myaxios.get('activity/all?currPage=1&pageSize=10').then((res) => {
        //if(res.status==1) 保留正确性测试
        //console.log("yes");
        return {tableData: res.data.result, tableLength: res.data.totalCount, loadings: false}
      }).catch((e) => {
        alert("请求错误！");
        return {loadings: false}
      })
    },
    methods: {
      handleClick(row, type) {
        const store = window.sessionStorage;//设置session缓存
        if (type == 2) {//历史活动
          store.setItem('is_history_' + row.activityId, '1');
        } else if (type == 1) {
          store.setItem('is_history_' + row.activityId, '0');
        }
        store.setItem('list_' + row.activityId, JSON.stringify(row));
        this.$router.push({path: '/describe?id=' + row.activityId});
      },
      goBack() {
        var urls = this.$route.query.url;
        this.$router.push({path: '/' + urls});
      },
      listChange(val) {
        myaxios.get("activity/all?currPage=" + val + "&pageSize=10").then((res) => {
          const store = window.sessionStorage;//设置session缓存
          this.tableData = res.data.result;//设置分页数据
        }).catch((e) => {
          this.$message.error('请求数据失败！请刷新页面重试');
          this.tableData = "";
        });
      },
      historyListChange(val) {
        const store = window.sessionStorage;
        myaxios.get('activity/group/' + store.getItem('user') + '/history?currPage=' + val + '&pageSize=10').then((res) => {
          this.historyData = res.data.result;
          this.historyTableLength = res.data.totalCount;
          this.loadings = false;
        }).catch((e) => {
          this.$message.error('请求历史数据失败！请刷新页面重试');
          this.loadings = false;
        });
      },
      autoClick() {
        const store = window.sessionStorage;
        myaxios.get('activity/group/' + store.getItem('user') + '/history?currPage=1&pageSize=10').then((res) => {
          this.historyData = res.data.result;
          this.historyTableLength = res.data.totalCount;
          this.loadings = false;
        }).catch((e) => {
          this.$message.error('请求历史数据失败！请刷新页面重试');
          this.loadings = false;
        });
      }
    },
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'}
        ],
        style: [],
        title: '活动列表'
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
        loadings: true,
        tableLength: 0,
        tableData: [],
        historyData: [],
        historyTableLength: 0,
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
