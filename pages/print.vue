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
          </div>
        </div>
        <el-alert title="请输入筛选条件以便搜索" type="info" description="您可以填写某一项或多项筛选条件，也可以选择不填写。搜索服务仅对您填写的部分进行范围查询。" show-icon>
        </el-alert>
        <div class="box"></div>
        <el-card shadow="hover">
          <div style="margin-top: 15px;">
            <el-form ref="form" :model="form">
              <el-divider>搜索</el-divider>
              <el-form-item prop="studentName">
                <el-input placeholder="请输入内容" v-model="form.studentName" class="input-with-select">
                  <template slot="prepend">学生姓名</template>
                </el-input>
              </el-form-item>
              <el-form-item prop="college">
                <el-input placeholder="请输入内容" v-model="form.college" class="input-with-select">
                  <template slot="prepend">所属学院</template>
                </el-input>
              </el-form-item>
              <el-form-item prop="highSchool">
                <el-input placeholder="请输入内容" v-model="form.highSchool" class="input-with-select">
                  <template slot="prepend">回访中学</template>
                </el-input>
              </el-form-item>
              <el-form-item label="申请状态">
                <el-select v-model="form.applyStatus" placeholder="请选择">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value">
                  </el-option>
                </el-select>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">搜索</el-button>
                <el-button type="primary" @click="exportData('ruleForm')">导出学生信息</el-button>
                <!--<el-button type="danger" @click="resetForm('ruleForm')">重置</el-button>-->
              </el-form-item>
            </el-form>
          </div>
        </el-card>
        <div class="box"></div>
        <el-card shadow="hover">
          <el-table border :data="tableData" style="width:100%" max-height="500" v-loading="loadings">
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

  const myaxios = axios.create({
    timeout: 3000,
    baseURL: "http://101.37.173.57:8080/",
    withCredentials: true,
  });

  export default {
    methods: {
      handleClick(row) {
        //console.log(row);
      },
      goBack() {
        this.$router.push({path: '/dashboard'});
      },
      exportData() {
        const store = window.sessionStorage;
        const userid = store.getItem('user');
        let url = 'apply/search-for-group/' + userid + '?currPage=1&pageSize=10';
        axios({
          method: 'post',
          withCredentials: true,
          url: 'http://101.37.173.57:8080/apply/export-for-group/' + userid,
          data: {
            studentName: this.form.studentName,
            college: this.form.college,
            highSchool: this.form.highSchool,
            applyStatus: this.form.applyStatus,
          },
          responseType: 'blob'
        }).then((res) => {
          this.$message.success('导出请求提交成功！');
          this.download(res.data);
        }).catch((e) => {
          this.$message.error('操作提交失败！请刷新页面重试');
        });
      },
      submitForm(form) {
        const store = window.sessionStorage;
        const userid = store.getItem('user');
        this.loadings = true;//打开加载效果
        let url = 'apply/search-for-group/' + userid + '?currPage=1&pageSize=10';
        let data = {
          studentName: this.form.studentName,
          college: this.form.college,
          highSchool: this.form.highSchool,
          applyStatus: this.form.applyStatus,
        };
        let jsonRequest = JSON.stringify(data);
        axios({
          method: 'post',
          withCredentials: true,
          url: 'http://101.37.173.57:8080/apply/search-for-group/' + userid + '?currPage=1&pageSize=10',
          data: jsonRequest,
          headers: {'Content-Type': 'application/json'},
        }).then((res) => {
          this.tableLength = res.data.totalCount;
          this.tableData = res.data.result;
          this.loadings = false;
        }).catch((e) => {
          this.$message.error('搜索加载失败，请重试或刷新页面！');
          this.loadings = false;
        });
      },
      handleCurrentChange(pages) {
        const store = window.sessionStorage;
        const userid = store.getItem('user');
        let data = {
          studentName: this.form.studentName,
          college: this.form.college,
          highSchool: this.form.highSchool,
          applyStatus: this.form.applyStatus,
        };
        let jsonRequest = JSON.stringify(data);
        axios({
          method: 'post',
          withCredentials: true,
          url: 'http://101.37.173.57:8080/apply/search-for-group/' + userid + '?currPage=' + pages + '&pageSize=10',
          data: jsonRequest,
          headers: {'Content-Type': 'application/json'},
        }).then((res) => {
          this.tableLength = res.data.totalCount;
          this.tableData = res.data.result;
          this.loadings = false;
        }).catch((e) => {
          this.$message.error('搜索加载失败，请重试或刷新页面！');
          this.loadings = false;
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      download(data) {
        let url = window.URL.createObjectURL(new Blob([data], {type: "application/vnd.ms-excel;charset=utf-8"}));
        let link = document.createElement('a');
        link.style.display = 'none';
        link.href = url;
        link.setAttribute('download', '学生信息.xls');
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
        title: '人员导出'
      }
    },
    data() {
      return {
        checkList: [],
        loadings: false,
        tableData: [],
        tableLength: 0,
        form: {
          studentName: null,
          college: null,
          highSchool: null,
          applyStatus: null,
          select: ''
        },
        options: [
          {
            value: 0,
            label: '未处理'
          },
          {
            value: 1,
            label: '已通过'
          },
          {
            value: 2,
            label: '未通过'
          }
        ],
        value: ''
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

  .el-select .input-with-select {
    width: 130px;
    height: 100px;
  }

  .input-with-select .el-input-group__prepend {
    background-color: #fff;
    width: 130px;
    height: 100px;
  }
</style>
