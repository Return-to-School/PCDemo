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
    <el-row type="flex" class="row-bg" justify="center" gutter="20">
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
          <el-table border :data="tableData" style="width:100%" max-height="500">
            <el-table-column fixed prop="id" label="活动编号">
            </el-table-column>
            <el-table-column prop="name" label="活动名称">
            </el-table-column>
            <el-table-column prop="date" label="活动日期">
            </el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button @click="handleClick(scope.row)" type="text" size="small">查看</el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-card>
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
      goBack(){
        this.$router.push({path:'/dashboard'});
      }
    },
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'}
        ],
        style: [],
        title:'活动列表'
      }
    },
    data() {
      return {
        tableData: [{
          id: '#00001',
          name: '第一个活动',
          date: '2020-01-20'
        },
          {
            id: '#00002',
            name: '第二个活动',
            date: '2020-01-21'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
          {
            id: '#00001',
            name: '第一个活动',
            date: '2020-01-20'
          },
        ]
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
