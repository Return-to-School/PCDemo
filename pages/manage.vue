<template>
  <div>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="18">
        <div class="grid-content">
          <el-button type="primary" round @click="goBack()">返回</el-button>
          <el-divider direction="vertical"></el-divider>
          <span>账号管理</span>
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
            <el-collapse-item title="创建账号" name="1">
              <div>您可以在此页面创建一个新的区域管理员账号，区域管理员账号仅能对其所属区域内的活动进行操作。</div>
            </el-collapse-item>
            <el-collapse-item title="修改账号" name="2">
              <div>您可以通过选择列表中的任何一个账号进行信息修改。</div>
            </el-collapse-item>
          </el-collapse>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-tabs type="border-card">
          <el-tab-pane label="创建账号">
            <div class="subtitle">
              <i class="el-icon-s-order"></i>
              创建账号
            </div>
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="80px" label-position="right">
              <el-form-item label="用户名" prop="username">
                <el-input v-model="ruleForm.username">
                </el-input>
              </el-form-item>
              <el-form-item label="密码" prop="password">
                <el-input v-model="ruleForm.password" show-password>
                </el-input>
              </el-form-item>
              <el-form-item label="管理区域" prop="area">
                <el-input v-model="ruleForm.area">
                </el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                <el-button type="danger" @click="resetForm('ruleForm')">重置</el-button>
              </el-form-item>
            </el-form>
          </el-tab-pane>
          <el-tab-pane label="修改账号">
            <div class="subtitle">
              <i class="el-icon-s-order"></i>
              修改账号
            </div>
          </el-tab-pane>
        </el-tabs>
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
      },
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.$message.success('提交成功！');
          } else {
            this.$message.error('您填写的表单有误，请再次检查后提交！');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      }
    },
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'}
        ],
        style: [],
        title: '活动列表',
      }
    },
    data(){
      return {
        ruleForm: {
          username: '',
          password: '',
          area: ''
        },
        rules: {
          username: [
            {required: true, message: '请输入用户名', trigger: 'blur'},
            {min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '请输入密码', trigger: 'blur'},
            {min: 8, max: 18, message: '长度在 8 到 18 个字符', trigger: 'blur'}
          ],
          area: [
            {required: true, message: '请输入管理区域', trigger: 'blur'}
          ]
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
</style>
