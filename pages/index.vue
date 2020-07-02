<template>
  <div>
    <el-row :gutter="20" justify="center" align="bottom">
      <el-col :span="6" :offset="9">
        <div class="grid-content">
          <el-card shadow="hover" v-loading="loadings">
            <div slot="header" class="clearfix" style="align-content: center">
              <span><strong>管理端登入</strong></span>
            </div>
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="70px" label-position="right">
              <el-form-item label="用户名" prop="username">
                <el-input v-model="ruleForm.username" placeholder="请输入" id="username">
                </el-input>
              </el-form-item>
              <el-form-item label="密码" prop="pwd">
                <el-input v-model="ruleForm.pwd" placeholder="请输入" id="pwd" show-password>
                </el-input>
              </el-form-item>
              <div class="box"></div>
              <div style="align-content: center">
                <el-button type="primary" style="width: 100%;" @click="submitForm('ruleForm')">登入</el-button>
              </div>
            </el-form>
          </el-card>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import axios from "axios";

  const myaxios = axios.create({
    baseURL: "http://101.37.173.57:8080/",
    timeout: 5000,
    withCredentials: true,
    headers: {
      //传入头数据
    }
  });

  export default {
    name: "login",
    methods: {
      submitForm(form) {
        this.$refs[form].validate((valid) => {
          if (valid) {
            this.loadings = true;
            let data = {
              userId: this.ruleForm.username,
              password: this.ruleForm.pwd,
            }
            myaxios.post('/user/verification', data).then((res) => {
              if (res.data.success) {
                //登入成功
                const store = window.sessionStorage;
                store.clear();//清空所有之前的数据
                store.setItem('user', this.ruleForm.username);
                this.$message.success('登入成功！');
                this.$router.push({path: '/dashboard'});
              } else {
                this.$message.error('登入失败！原因：' + res.data.msg + '。');
                this.loadings = false;
              }
            }).catch((err) => {
              this.$message.error('登入失败！服务器响应错误，请重试！');
              console.log(err);
              this.loadings = false;
            })
          }
        });
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
        loadings: false,
        ruleForm: {
          username: '',
          pwd: '',
        },
        rules: {
          username: [
            {required: true, message: '用户名不能为空！', trigger: 'blur'},
          ],
          pwd: [
            {required: true, message: '密码不能为空！', trigger: 'blur'},
          ],
        }
      };
    },
  }
</script>

<style scoped>
  .grid-content {
    margin-top: 40%;
    min-height: 36px;
  }

  .box {
    height: 10px;
  }
</style>
