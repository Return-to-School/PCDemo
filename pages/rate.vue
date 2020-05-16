<template>
  <div>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="18">
        <div class="grid-content">
          <el-button type="primary" round @click="goBack()">返回</el-button>
          <el-divider direction="vertical"></el-divider>
          <span>学生资料</span>
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
            <el-collapse-item title="关于评分" name="1">
              <el-alert type="error" title="请注意：您在本页面对该学生的评分可能会影响其综合评分" :closable="false"></el-alert>
              <div>评分方式采用星级制。请您点击右上角"考评"按钮，并在弹出的对话框中使用鼠标点击星星进行评分。<strong>当前活动对该学生的评分能够且仅能够进行一次。</strong></div>
            </el-collapse-item>
            <el-collapse-item title="其他信息" name="2">
              <div>等待更新。</div>
            </el-collapse-item>
          </el-collapse>
        </el-card>
      </el-col>
      <el-col :span="12">
        <div style="display: inline">
          <div class="subtitle">
            <i class="el-icon-user-solid"></i>
            学生资料
            <div style="float:right" v-if="can==1">
              <el-button-group>
                <el-button type="primary" download="" href="" @click="downloadModal=true">下载学生反馈材料</el-button>
                <el-button type="primary" @click="openRate">考评</el-button>
              </el-button-group>
            </div>
          </div>
        </div>
        <div class="box"></div>
        <el-card shadow="hover" class="content">
          <div slot="header" class="clearfix">
            <span><strong>基本信息</strong></span>
          </div>
          <el-row type="flex" class="row-bg" justify="space-around">
            <el-col :span="8">
              <div class="grid-content">
                姓名：
                <el-tag type="">{{studentName}}</el-tag>
              </div>
            </el-col>
            <el-col :span="8">
              <div class="grid-content">
                生源地：
                <el-tag type="info">{{origin}}</el-tag>
              </div>
            </el-col>
          </el-row>
          <el-row type="flex" class="row-bg" justify="space-around">
            <el-col :span="8">
              <div class="grid-content">
                毕业高中：
                <el-tag type="success">{{highSchool}}</el-tag>
              </div>
            </el-col>
            <el-col :span="8">
              <div class="grid-content">
                学号：
                <el-tag type="danger">{{studentCard}}</el-tag>
              </div>
            </el-col>
          </el-row>
        </el-card>
        <div class="box"></div>
        <el-card shadow="hover">

        </el-card>
      </el-col>
    </el-row>
    <el-dialog title="考评" :visible.sync="rate" width="30%">
      <el-form ref="form" :model="form">
        <el-form-item label="评分">
          <el-radio-group v-model="form.radio">
            <el-radio-button label="2">优秀</el-radio-button>
            <el-radio-button label="1">合格</el-radio-button>
            <el-radio-button label="0">不合格</el-radio-button>
          </el-radio-group>
        </el-form-item>
        <el-divider></el-divider>
        <!--<el-form-item label="反馈资料" prop="upload">
          <el-upload
            class="upload"
            ref="upload"
            name="feedbackFiles"
            :action="uploadUrl"
            :on-preview="handlePreview"
            :on-remove="handleRemove"
            :before-remove="beforeRemove"
            :on-success="handleSuccess"
            multiple
            :file-list="fileList">
            <el-button size="small" type="primary">上传文件</el-button>
            <div slot="tip" class="el-upload__tip">（可选项）只能上传word/excel文件，且不超过500kb。选择文件后将自动上传！</div>
          </el-upload>
        </el-form-item>-->
        <el-divider></el-divider>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="submitForm('form')">提交反馈</el-button>
      </div>
    </el-dialog>
    <el-dialog title="考评" :visible.sync="downloadModal" width="30%">
      <el-table border :data="fileLists" style="width:100%" max-height="500" v-loading="loadings">
        <el-table-column fixed prop="id" label="文件名称">
        </el-table-column>
        <el-table-column label="操作">
          <template slot-scope="scope">
            <a @click="handleClick(scope.row)" type="text" size="small">下载</a>
          </template>
        </el-table-column>
      </el-table>
    </el-dialog>
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
        const id = this.$route.query.id;
        const store = window.sessionStorage;
        if (store.getItem('feedback_' + id) == null) {
          $('.content').text();//将页面内容设置为空，截停查询操作
          alert("很抱歉，您的提交参数有误，请重新操作进入本页面！");
          this.$router.push({path: '/list?url=dashboard'}); //直接将页面退出
        } else {
          const fbData = JSON.parse(store.getItem('feedback_' + id));
          const s = fbData.student;
          this.studentName = s.name;
          this.origin = fbData.origin;
          this.studentCard = s.studentCard;
          this.highSchool = s.highSchool;
          myaxios.get('feedback/' + id).then((res) => {
            if (res.data == null) {
              this.can = 2;//标记无反馈
            } else {
              this.feedbackUrl = res.data.filePath;
              this.feedbackId = res.data.apply;
            }
          })
        }
      },
      handleClick(row) {
        console.log(row);
      },
      downloadClick() {

      },
      goBack() {
        this.$router.push({path: '/describe?id=' + this.$route.query.id});
      },
      submitForm(form) {
        const id = this.$route.query.id;//查询url嵌入的当前申请id
        myaxios.get('feedback/' + id, {
          params: {
            level: this.form.radio
          }
        }).then((res) => {
          this.$message.success('提交成功！');
        }).catch((err) => {
          this.$message.error('提交失败！');
        });
      },
      handleSuccess(response, file, fileList) {
        //上传文件成功时
        this.$message.success('上传文件成功！');
      },
      handleRemove(file, fileList) {
        console.log(file, fileList);
      },
      handlePreview(file) {
        console.log(file);
      },
      handleExceed(files, fileList) {
        this.$message.warning(`本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
      },
      beforeRemove(file, fileList) {
        return this.$confirm(`确定移除 ${file.name}？`);
      },
      openRate() {
        const id = this.$route.query.id;//查询url嵌入的当前申请id
        this.uploadUrl = "http://101.37.173.57:8080/feedback/" + id;
        this.rate = true;
      }
    },
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'}
        ],
        style: [],
        title: '学生资料'
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
        can: 0,//是否有这个反馈 0暂时未判断 1有 2无
        feedbackId: 0,
        feedbackUrl: '',//反馈文件路径
        downloadModal: false,
        checkList: [],
        rate: false,
        form: {
          radio: []
        },
        fileList: [],
        fileLists: [],
        uploadData: [],
        downloadUrl: '',
        uploadUrl: "",
        studentName: "",
        studentCard: 0,
        classNo: 0,
        major: "",
        origin: "",
        highSchool: "",

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
