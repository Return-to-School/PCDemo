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
      <el-col :span="4">
        <el-menu default-active="1">
          <el-menu-item index="1">
            <i class="el-icon-menu"></i>
            <span slot="title">所有活动</span>
          </el-menu-item>
          <el-menu-item index="2">
            <i class="el-icon-menu"></i>
            <span slot="title">待定内容</span>
          </el-menu-item>
        </el-menu>
      </el-col>
      <el-col :span="14">
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
                <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
              </template>
            </el-table-column>
          </el-table>
          <div style="margin-top:20px">
            <el-pagination background layout="total, prev, pager, next" :total="tableLength"
                           @current-change="handleCurrentChange">
            </el-pagination>
          </div>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    asyncData() { //进入页面前的预加载
      const myaxios = axios.create({
        baseURL: "http://101.37.173.57:8080/",
        timeout: 5000  //设置5秒的超时
      });
      myaxios.interceptors.request.use(function (config) {
        return config;
      });
      return myaxios.get('activity/all?currPage=1&pageSize=10').then((res) => {
        //if(res.status==1) 保留正确性测试
        console.log("yes");
        return {tableData: res.data.result, tableLength: res.data.totalCount, loadings: false}
      }).catch((e) => {
        //this.$message.error('请求数据失败！请刷新页面重试');
        console.log("请求错误！");
        return {loadings: false}
      })
    },
    methods: {
      handleClick(row) {
        const store = window.sessionStorage;//设置session缓存
        //if (store.getItem('list_' + row.id) == null)
        store.setItem('list_' + row.activityId, JSON.stringify(row));//将用户指定的list对象放入session并保持最新
        console.log(row);
        this.$router.push({path: '/describe?id=' + row.activityId});
      },
      goBack() {
        var urls = this.$route.query.url;
        this.$router.push({path: '/' + urls});
      },
      handleCurrentChange(val) {
        const myaxios = axios.create({
          baseURL: "http://101.37.173.57:8080/",
          timeout: 5000,
          headers: {
            //传入头数据
          }
        });
        myaxios.get("activity/all?currPage=" + val + "&pageSize=10").then((res) => {
          const store = window.sessionStorage;//设置session缓存
          this.tableData = res.data.result;//设置分页数据
        }).catch((e) => {
          this.$message.error('请求数据失败！请刷新页面重试');
          this.tableData = "";
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
    data() {
      return {
        loadings: true,
        tableLength: 100,
        tableData: []
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
