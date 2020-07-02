<template>
  <div>
    <el-row type="flex" class="row-bg" justify="center">
      <el-col :span="18">
        <div class="grid-content">
          <el-button type="primary" round @click="goBack()">返回</el-button>
          <el-divider direction="vertical"></el-divider>
          <span v-if="newForm">新建活动</span>
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
          </el-collapse>
        </el-card>
      </el-col>
      <el-col :span="12">
        <div style="display: inline">
          <div class="subtitle" v-if="newForm">
            <i class="el-icon-plus"></i>
            新建活动
          </div>
          <div class="subtitle" v-else>
            <i class="el-icon-edit-outline"></i>
            修改活动
          </div>
        </div>
        <div class="box"></div>
        <el-alert title="修改时请注意：" type="warning" description="活动区域必须重新选择，否则无法提交！" show-icon v-if="!newForm">
        </el-alert>
        <el-card shadow="hover">
          <div class="input-box">
            <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="140px" label-position="right">
              <el-divider>活动基本信息</el-divider>
              <el-form-item label="活动名称" prop="name">
                <el-input v-model="ruleForm.name" placeholder="请输入" id="name">
                </el-input>
              </el-form-item>
              <el-form-item label="活动创建者" prop="name">
                <el-input v-model="ruleForm.creator" placeholder="请输入" id="creator">
                </el-input>
              </el-form-item>
              <el-form-item label="活动区域" prop="location">
                <div class="block">
                  <el-cascader v-model="ruleForm.location" :options="options" :props="{ emitPath:false }"
                               separator="-" :placeholder="ruleForm.location"
                               clearable :key="changeOptions" ref="locations"
                               @change="handleChange" @visible-change="openCascader" @expand-change="changeExpand"
                               id="location">
                  </el-cascader>
                </div>
              </el-form-item>
              <el-form-item label="活动申请起止时间" prop="applyTime">
                <el-date-picker v-model="ruleForm.applyTime" type="datetimerange" range-separator="至"
                                start-placeholder="开始日期" end-placeholder="结束日期" value-format="yyyy-MM-dd HH:mm:ss">
                </el-date-picker>
              </el-form-item>
              <el-form-item label="提交反馈起止时间" prop="feedbackTime">
                <el-date-picker v-model="ruleForm.feedbackTime" type="datetimerange" range-separator="至"
                                start-placeholder="开始日期" end-placeholder="结束日期" value-format="yyyy-MM-dd HH:mm:ss">
                </el-date-picker>
              </el-form-item>
              <el-divider>活动详细信息</el-divider>
              <el-form-item label="材料清单" prop="upload">
                <el-upload
                  class="upload"
                  ref="uploads"
                  name="activityFiles"
                  :action="uploadUrl"
                  :on-success="handleSuccess"
                  :on-error="handleError"
                  :on-preview="handlePreview"
                  :on-remove="handleRemove"
                  :before-remove="beforeRemove"
                  multiple
                  :auto-upload="false"
                  :file-list="fileList">
                  <el-button size="small" type="primary">点击上传</el-button>
                  <div slot="tip" class="el-upload__tip">只能上传word/excel文件，且不超过500kb</div>
                </el-upload>
              </el-form-item>
              <el-form-item label="活动介绍" prop="desc" style="height: 290px;">
                <quill-editor ref="text" v-model="ruleForm.desc" style="height: 150px;"
                              :options="ruleForm.editorOption">
                  <div id="toolbar" slot="toolbar">
                    <!-- 重新定义文本编辑器样式 -->
                    <button class="ql-bold" title="加粗">Bold</button>
                    <button class="ql-italic" title="斜体">Italic</button>
                    <button class="ql-underline" title="下划线">underline</button>
                    <button class="ql-strike" title="删除线">strike</button>
                    <button class="ql-blockquote" title="引用"></button>
                    <button class="ql-code-block" title="代码"></button>
                    <button class="ql-header" value="1" title="标题1"></button>
                    <button class="ql-header" value="2" title="标题2"></button>
                    <!--Add list -->
                    <button class="ql-list" value="ordered" title="有序列表"></button>
                    <button class="ql-list" value="bullet" title="无序列表"></button>
                    <!-- Add font size dropdown -->
                    <select class="ql-header" title="段落格式">
                      <option selected>段落</option>
                      <option value="1">标题1</option>
                      <option value="2">标题2</option>
                      <option value="3">标题3</option>
                      <option value="4">标题4</option>
                      <option value="5">标题5</option>
                      <option value="6">标题6</option>
                    </select>
                    <select class="ql-size" title="字体大小">
                      <option value="10px">10px</option>
                      <option value="12px">12px</option>
                      <option value="14px">14px</option>
                      <option value="16px" selected>16px</option>
                      <option value="18px">18px</option>
                      <option value="20px">20px</option>
                      <option value="40px">40px</option>
                    </select>
                    <select class="ql-font" title="字体">
                      <option value="SimSun">宋体</option>
                      <option value="SimHei">黑体</option>
                      <option value="Microsoft-YaHei">微软雅黑</option>
                      <option value="KaiTi">楷体</option>
                      <option value="FangSong">仿宋</option>
                      <option value="Arial">Arial</option>
                    </select>
                    <!-- Add subscript and superscript buttons -->
                    <select class="ql-color" value="color" title="字体颜色"></select>
                    <select class="ql-background" value="background" title="背景颜色"></select>
                    <select class="ql-align" value="align" title="对齐"></select>
                    <button class="ql-clean" title="还原"></button>
                    <!-- You can also add your own -->
                  </div>
                </quill-editor>
                <!--<el-input type="textarea" v-model="ruleForm.desc" placeholder="请输入" id="desc"></el-input>-->
              </el-form-item>
              <el-form-item label="是否需要审核" prop="verify">
                <el-switch v-model="ruleForm.verify" active-color="#13ce66" inactive-color="#ff4949"></el-switch>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">
                  <div v-if="newForm">立即创建</div>
                  <div v-else>修改信息</div>
                </el-button>
                <el-button type="danger" @click="resetForm('ruleForm')">重置</el-button>
              </el-form-item>
            </el-form>
          </div>
        </el-card>
      </el-col>
    </el-row>
    <el-button v-trigger @click="autoClick" style="visibility: hidden;">
    </el-button>
  </div>
</template>

<script>
  import axios from 'axios'
  import {
    quillEditor,
    Quill
  } from 'vue-quill-editor';//按需引入富文本组件
  import 'quill/dist/quill.core.css'
  import 'quill/dist/quill.snow.css'

  //引入font.css
  import '../assets/quillFont.css'

  // 自定义字体大小
  let Size = Quill.import('attributors/style/size');
  Size.whitelist = ['10px', '12px', '14px', '16px', '18px', '20px', '40px'];
  Quill.register(Size, true);

  // 自定义字体类型
  var fonts = ['SimSun', 'SimHei', 'Microsoft-YaHei', 'KaiTi', 'FangSong', 'Arial', 'Times-New-Roman', 'sans-serif',
    '宋体', '黑体'
  ];
  var Font = Quill.import('formats/font');
  Font.whitelist = fonts;
  Quill.register(Font, true);

  const myaxios = axios.create({
    timeout: 3000,
    baseURL: "http://101.37.173.57:8080/",
    withCredentials: true,
  });

  export default {
    components: {
      quillEditor
    },
    methods: {
      handleClick(row) {
      },
      goBack() {
        var urls = this.$route.query.url;
        if (urls == "describe") {
          this.$router.push({path: '/' + urls + '?id=' + this.$route.query.sign});
        } else {
          this.$router.push({path: '/' + urls});
        }
      },
      submitForm(formName) {
        const store = window.sessionStorage;
        const userid = store.getItem('user');
        this.$refs[formName].validate((valid) => {
          if (valid) {
            if (this.ruleForm.feedbackTime[0] <= this.ruleForm.applyTime[1]) {
              //如果反馈开始时间早于申请结束时间
              this.$message.error('反馈开始时间必须晚于申请结束时间!');
              return;
            }
            let url = '';
            let data_l = {};
            if (this.newForm) {
              //如果是新建
              url = 'activity/' + userid;
              data_l = {
                name: this.ruleForm.name,
                creator: this.ruleForm.creator,
                content: this.ruleForm.desc,
                applyStartTime: this.ruleForm.applyTime[0],
                applyEndTime: this.ruleForm.applyTime[1],
                feedbackStartTime: this.ruleForm.feedbackTime[0],
                feedbackEndTime: this.ruleForm.feedbackTime[1],
                needExamine: this.ruleForm.verify,
                location: this.finalResult
              };
              myaxios.post(url, data_l).then((res) => {
                if (res.data.success) {
                  $('.input-box').html(
                    "<h2 class=\"subtitle\" style='text-align: center;color:green'>\n" +
                    "            <i class=\"el-icon-circle-check\"></i>\n" +
                    "            提交成功！\n" +
                    "          </h2>");
                  if (res.data.activityId != -1) {
                    this.uploadUrl = "http://101.37.173.57:8080/activity/file/" + res.data.activityId;
                    let _this = this;
                    setTimeout(function () {
                      _this.$refs.uploads.submit();
                    }, 400);//利用延时来保证文件提交的正确
                  } else {
                    this.$message.error('活动创建失败，文件无法上传!');
                  }
                  this.$message.success('提交成功！');
                } else {
                  this.$message.error('提交失败，失败原因：' + res.data.msg);
                }
              }).catch((e) => {
                this.$message.error('提交失败，请尝试刷新页面或联系网站维护人员处理！');
              });
            } else {
              let activityid = this.$route.query.sign;
              url = 'activity/' + this.$route.query.sign;
              data_l = {
                name: this.ruleForm.name,
                creator: this.ruleForm.creator,
                content: this.ruleForm.desc,
                applyStartTime: this.ruleForm.applyTime[0],
                applyEndTime: this.ruleForm.applyTime[1],
                feedbackStartTime: this.ruleForm.feedbackTime[0],
                feedbackEndTime: this.ruleForm.feedbackTime[1],
                needExamine: this.ruleForm.verify,
                location: this.finalResult,
                activityId: activityid
              };
              myaxios.put(url, data_l).then((res) => {
                if (res.data.success) {
                  store.setItem('list_' + activityid, null);//安全起见，清空原来的session
                  $('.input-box').html(
                    "<h2 class=\"subtitle\" style='text-align: center;color:green'>\n" +
                    "            <i class=\"el-icon-circle-check\"></i>\n" +
                    "            修改成功！\n" +
                    "          </h2>");
                  this.$message.success('修改成功！');
                  this.updateSession();//刷新新的session
                  this.uploadUrl = "http://101.37.173.57:8080/activity/file/" + this.$route.query.sign;
                  let _this = this;
                  setTimeout(function () {
                    _this.$refs.uploads.submit();
                  }, 400);
                } else {
                  this.$message.error('修改失败，请重新尝试！');
                }
              }).catch((e) => {
                this.$message.error('修改失败，请尝试刷新页面或联系网站维护人员处理！');
              });
            }
          } else {
            this.$message.error('您填写的表单有误，请再次检查后提交！');
            return false;
          }
        });
      },
      updateSession() {
        let activityId = this.$route.query.sign;
        const store = window.sessionStorage;
        const userid = store.getItem('user');
        myaxios.post('/apply/search-for-group/' + userid + '?currPage=1' + '&pageSize=10', {
          activityId: this.$route.query.sign
        }).then((res) => {
          console.log(res.data.result[0].activity);
          store.setItem('list_' + activityId, JSON.stringify(res.data.result[0].activity));
        }).catch((e) => {
          //加载错误中断请求
          store.setItem('list_' + activityId, null);
          this.$message.error('请求数据失败！请刷新页面重试');
        });
      },
      handleChange(val) {
        //处理地区选择栏的选择，其实就是将最后的区属性加入最终数据中
        if (val == '-1') {
          this.finalResult = this.locationResult;//如果是两级地区就直接将最后一个项去掉
        } else {
          this.finalResult = this.locationResult + '-' + val;//否则就将最后一项加上去
        }
      },
      openCascader(status) {
        //处理地区选择器的第一次请求
        if (status && !this.p) {//当前操作为打开下拉菜单(只渲染一次！！！！)
          this.options = [{
            value: 233,
            label: '加载中...',
            children: []
          }];
          this.p++;
          var result = [], now, id, foot;//初始化一个数组准备注入
          myaxios.get('location/all-provinces').then((res) => {
            //递归设置地区
            res.data.forEach((item, index, arr) => {
              id = item.provinceId;//获取省份id
              foot = result.length;//获取当前应该插入的下标位置
              now = {
                value: item.name + "+1+" + item.provinceId + "+" + foot,//格式：名称+1+省份编码
                label: item.name,
                children: []
              };
              result.push(now);
            });
            this.options = result;//注入数据
          }).catch((err) => {
            this.$message.error("地区获取失败，请重试！");
          });
        }
      },
      changeExpand(val) {
        //选择选项时触发，返回value
        var now = {};
        if (val.length == 1) {
          //如果是城市
          const data = val[0].split('+');
          const id = data[2], type = data[1], foot = data[3];
          myaxios.get('location/cities/' + id).then((res) => {
            res.data.forEach((item, index, arr) => {
              now = {
                value: item.name + "+2+" + item.cityId + "+" + foot + "+" + (this.options)[foot].children.length, //城市名称+类型+城市id+省份下标+城市下标
                label: item.name,
                children: []
              };
              (this.options)[foot].children.push(now); //注入城市数据
            })
          })
        } else {
          //获取区信息
          this.locationResult = (val[0].split("+"))[0] + "-" + (val[1].split("+"))[0];//拼接省份和城市的地址串
          const data = val[1].split("+");
          const id = data[2], type = data[1], proFoot = data[3], cityFoot = data[4], cityName = data[0];
          myaxios.get('location/counties/' + id).then((res) => {
            if (res.data.length == 0) {
              //处理只有两级的省份，就将城市名字再注入一次，最后提交的时候检测一下
              console.log('这是只有两级的省份。');
              now = {
                value: '-1',//标识-1表示是两级省份
                label: cityName
              };
              ((this.options)[proFoot].children)[cityFoot].children.push(now); //注入地区数据
            }
            res.data.forEach((item, index, arr) => {
              now = {
                value: item.name, //城市名称+类型+城市id+省份下标+城市下标
                label: item.name,
              };
              ((this.options)[proFoot].children)[cityFoot].children.push(now); //注入地区数据
            })
          })
        }
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      handleRemove(file, fileList) {
        //console.log(file, fileList);
      },
      handlePreview(file) {
        //console.log(file);
      },
      handleExceed(files, fileList) {
        this.$message.warning(`本次选择了 ${files.length} 个文件，共选择了 ${files.length + fileList.length} 个文件`);
      },
      beforeRemove(file, fileList) {
        return this.$confirm(`确定移除 ${file.name}？`);
      },
      handleSuccess(response, file, fileList) {
        this.$message.success("文件上传成功！");
      },
      handleError() {
        this.$message.error("文件上传失败！请在修改活动信息页面重新上传");
      },
      autoClick() {
        //加载页面之前对页面进行处理
        const action = this.$route.query.action;
        if (action == "new") {
          this.newForm = true;
        } else if (action == "update") {
          //修改活动操作
          const id = this.$route.query.sign;
          const store = window.sessionStorage;
          if (store.getItem('update_' + id) != null) {
            //判断当前修改操作合法性
            const data = JSON.parse(store.getItem('update_' + id));
            this.ruleForm.name = data.name;
            this.ruleForm.desc = data.content;
            this.ruleForm.creator = data.creator;
            this.ruleForm.applyTime = [data.applyStartTime, data.applyEndTime];
            this.ruleForm.feedbackTime = [data.feedbackStartTime, data.feedbackEndTime];
            this.ruleForm.verify = data.needExamine;
            //this.ruleForm.location = data.location;
            this.newForm = false;//更改表单属性为修改模式
          } else {
            alert("很抱歉，您的提交参数有误，请重新操作进入本页面！");
            this.$router.push({path: '/list?url=dashboard'}); //直接将页面退出
          }
        }
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
    directives: {
      trigger: {
        inserted(el, b) {
          el.click();
        }
      }
    },
    data() {
      return {
        p: 0,
        uploadUrl: '',//创建提交路径
        changeOptions: 0,//用于防止地区选择器报错
        options: [],//地区信息实时更新
        locationResult: "",//存入选择的地区省区路径
        finalResult: "",
        newForm: true,
        fileList: [],
        updateForm: {
          //修改参数默认值
          name: '',
          creator: '',
          desc: '',
          location: '请选择',
          verify: false
        },
        ruleForm: {
          name: '',
          applyTime: '',
          feedbackTime: '',
          location: [],
          desc: '',
          editorOption: {
            placeholder: '请输入内容',
            theme: "snow",
            modules: {
              toolbar: {
                container: '#toolbar'
              }
            }
          },
          verify: true, //是否需要审核
        },
        rules: {
          name: [
            {required: true, message: '请输入活动名称', trigger: 'blur'},
            {min: 5, max: 15, message: '长度在 5 到 15 个字符', trigger: 'blur'}
          ],
          applyTime: [
            {required: true, message: '请选择活动起止时间', trigger: 'change'}
          ],
          feedbackTime: [
            {required: true, message: '请选择资料上传起止时间', trigger: 'change'}
          ],
          desc: [
            {required: true, message: '请填写活动介绍', trigger: 'blur'}
          ],
          location: [
            {required: true, message: '请选择活动地区', trigger: 'blur'}
          ]

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
