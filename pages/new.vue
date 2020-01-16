<template>
  <div>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="18">
        <div class="grid-content">
          <el-button type="primary" round @click="goBack()">返回</el-button>
          <el-divider direction="vertical"></el-divider>
          <span v-if="isnew">新建活动</span>
          <span v-else>修改活动</span>
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
            <el-collapse-item title="递交材料设置" name="1">
              <div>若您的活动需要指定报名者准备材料，请事先编辑材料清单并保存为word文档格式，点击表单内"上传材料清单"按钮上传</div>
            </el-collapse-item>
            <el-collapse-item title="活动人数设置" name="2">
              <div>您可以通过表单内的"活动人数设置"设置活动的最大有效人数。</div>
              <el-alert type="error" title="请注意：您必须通过申请者递交的申请才能增加当前活动有效人数" :closable="false"></el-alert>
            </el-collapse-item>
          </el-collapse>
        </el-card>
      </el-col>
      <el-col :span="12">
        <div style="display: inline">
          <div class="subtitle" v-if="isnew">
            <i class="el-icon-plus"></i>
            新建活动
          </div>
          <div class="subtitle" v-else>
            <i class="el-icon-edit-outline"></i>
            修改活动
          </div>
        </div>
        <div class="box"></div>
        <el-card shadow="hover">
          <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="140px" label-position="right">
            <el-divider>活动基本信息</el-divider>
            <el-form-item label="活动名称" prop="name">
              <el-input v-model="ruleForm.name">
              </el-input>
            </el-form-item>
            <el-form-item label="活动起止时间" prop="activityTime">
              <el-date-picker v-model="ruleForm.activityTime" type="datetimerange" range-separator="至"
                              start-placeholder="开始日期" end-placeholder="结束日期">
              </el-date-picker>
            </el-form-item>
            <el-form-item label="材料上传起止时间" prop="uploadTime">
              <el-date-picker v-model="ruleForm.uploadTime" type="datetimerange" range-separator="至"
                              start-placeholder="开始日期" end-placeholder="结束日期">
              </el-date-picker>
            </el-form-item>
            <el-divider>活动详细信息</el-divider>
            <el-form-item label="活动介绍" prop="desc">
              <el-input type="textarea" v-model="ruleForm.desc"></el-input>
            </el-form-item>
            <el-form-item label="是否需要审核" prop="verify">
              <el-switch v-model="ruleForm.verify" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
              <el-button @click="resetForm('ruleForm')">重置</el-button>
            </el-form-item>
          </el-form>
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
        var urls = this.$route.query.url;
        this.$router.push({path: '/' + urls});
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
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'},
        ],
        title: '新建活动'
      }
    },
    data() {
      return {
        isnew: true,
        ruleForm: {
          name: '',
          activityTime: '',
          uploadTime: '',
          desc: '',
          verify: true,
        },
        rules: {
          name: [
            {required: true, message: '请输入活动名称', trigger: 'blur'},
            {min: 5, max: 15, message: '长度在 5 到 15 个字符', trigger: 'blur'}
          ],
          activityTime: [
            {required: true, message: '请选择活动起止时间', trigger: 'change'}
          ],
          uploadTime: [
            {required: true, message: '请选择资料上传起止时间', trigger: 'change'}
          ],
          desc: [
            {required: true, message: '请填写活动介绍', trigger: 'blur'}
          ],

        }
      }
    }
  }
</script>

<style scoped>
  .box {
    height: 10px;
  }

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
</style>
