<template>
  <div>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="18">
        <div class="grid-content">
          <el-button type="primary" round @click="goBack()">返回</el-button>
          <el-divider direction="vertical"></el-divider>
          <span>人员导出</span>
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
            <el-collapse-item title="关于搜索关键字" name="1">
              <div>您在搜索框中输入的内容会作为人员姓名关键字进行匹配，从而导出指定人员的信息。当然，您也可以不填写任何内容，系统将默认为您导出所有人员信息。</div>
            </el-collapse-item>
            <el-collapse-item title="关于搜索时间过长" name="2">
              <div>搜索内容所需时间很大程度上受数据大小影响，若数据量过大可能会需要更多的等待时间。若系统长时间未反应，请您考虑刷新页面或者寻求网站运营方的帮助。</div>
            </el-collapse-item>
          </el-collapse>
        </el-card>
      </el-col>
      <el-col :span="12">
        <div style="display: inline">
          <div class="subtitle">
            <i class="el-icon-user-solid"></i>
            人员导出
            <div style="float:right">
              <el-button-group>
                <el-button type="primary">导出学生信息</el-button>
              </el-button-group>
            </div>
          </div>
        </div>
        <el-alert title="请输入姓名关键字进行搜索" type="info" description="您也可以不进行任何操作并点击右上角的按钮，系统将为您导出所有人员信息。" show-icon>
        </el-alert>
        <div class="box"></div>
        <el-card shadow="hover">
          <div style="margin-top: 15px;">
            <el-form ref="form" :model="form">
              <el-form-item>
                <el-input placeholder="请输入内容" v-model="form.search">
                  <el-button slot="append" icon="el-icon-search"></el-button>
                </el-input>
              </el-form-item>
            </el-form>
          </div>
        </el-card>
        <div class="box"></div>
        <el-card shadow="hover">
          <el-table border :data="tableData" style="width:100%" max-height="500">
            <el-table-column fixed prop="id" label="姓名">
            </el-table-column>
            <el-table-column prop="name" label="属性">
            </el-table-column>
            <el-table-column prop="date" label="属性">
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
      goBack() {
        this.$router.push({path: '/dashboard'});
      }
    },
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'}
        ],
        style: [],
        title: '人员导出'
      }
    },
    data() {
      return {
        form:{
          search:''
        }
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

  .el-select .el-input {
    width: 130px;
  }

  .input-with-select .el-input-group__prepend {
    background-color: #fff;
  }
</style>
