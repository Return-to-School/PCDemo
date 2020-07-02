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
          </el-collapse>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-tabs type="border-card">
          <!--<el-tab-pane label="创建账号">
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
              <el-form-item label="活动区域" prop="area">
                <div class="block">
                  <el-cascader class="areaChoose" v-model="chooseValue" :options="options" :props="{ emitPath:false }" separator="-"
                               :key="changeOptions"
                               @change="handleChange" @visible-change="openCascader" @expand-change="changeExpand">
                  </el-cascader>
                </div>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">立即创建</el-button>
                <el-button type="danger" @click="resetForm('ruleForm')">重置</el-button>
              </el-form-item>
            </el-form>
          </el-tab-pane>-->
          <el-tab-pane label="修改账号" @click="handleCurrentChange(1)">
            <div class="subtitle">
              <i class="el-icon-s-order"></i>
              修改账号
            </div>
            <el-table border :data="tableData" style="width:100%" max-height="500" v-loading="loadings">
              <el-table-column prop="userId" label="UserId">
              </el-table-column>
              <el-table-column prop="role.desc" label="权限">
              </el-table-column>
              <el-table-column prop="loc" label="管理区域">
              </el-table-column>
              <!--<el-table-column label="操作">
                <template slot-scope="scope">
                  <el-button @click="openPwdDlg(scope.row)" type="text" size="small">修改</el-button>
                </template>
              </el-table-column>-->
            </el-table>
            <div style="margin-top:20px">
              <el-pagination background layout="total, prev, pager, next" :total="tableLength"
                             @current-change="handleCurrentChange">
              </el-pagination>
            </div>
          </el-tab-pane>
        </el-tabs>
      </el-col>
    </el-row>
    <el-button v-trigger @click="handleCurrentChange(1)" style="visibility: hidden;">
    </el-button>
  </div>
</template>

<script>
  import axios from "axios";

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
      openPwdDlg(row) {
        this.updateId = row.userId;
        this.updatePwdDlg = true;
      },
      handleCurrentChange(val) {
        this.loadings = true;
        myaxios.get("user/all?currPage=" + val + "&pageSize=10").then((res) => {
          this.tableLength = res.data.totalCount;
          this.tableData = res.data.result;//设置分页数据
          this.loadings = false;
        }).catch((e) => {
          this.$message.error('请求数据失败！请刷新页面重试');
          this.tableData = "";
          this.loadings = false;
        });
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
      handleChange(val) {
        //处理地区选择栏的选择
        //console.log(val);
      },
      openCascader(status) {
        if (status && !this.p) {//当前操作为打开下拉菜单(只渲染一次！！！！)
          this.options = [{
            value: 233,
            label: '加载中...',
            children: []
          }];
          this.p++;
          var result = [], now, id, foot;//初始化一个数组准备注入
          //两层递归
          myaxios.get('location/all-provinces').then((res) => {
            //递归设置地区
            res.data.forEach((item, index, arr) => {
              id = item.id;//获取省份id
              foot = result.length;//获取当前应该插入的下标位置
              now = {
                value: item.name + "+1+" + item.id + "+" + foot,//格式：名称+1+省份编码
                label: item.name,
                children: []
              };
              result.push(now);
            });
            console.log(result);
            this.options = result;//注入数据
          }).catch((err) => {
            this.$message.error("地区获取失败，请重试！");
          });
        }
      },
      changeExpand(val) {
        //选择选项时触发，返回value
        const data = val[0].split('+');
        var now = {};
        if (val.length == 1) {
          //如果是省份
          const data = val[0].split('+');
          const id = data[2], type = data[1], foot = data[3];
          myaxios.get('location/cities/' + id).then((res) => {
            res.data.forEach((item, index, arr) => {
              now = {
                value: item.name + "+2+" + item.id + "+" + foot + "+" + (this.options)[foot].children.length, //城市名称+类型+城市id+省份下标+城市下标
                label: item.name,
                children: []
              };
              (this.options)[foot].children.push(now); //注入城市数据
            })
          })
        } else {
          const data = val[1].split("+");
          const id = data[2], type = data[1], proFoot = data[3], cityFoot = data[4];
          myaxios.get('location/counties/' + id).then((res) => {
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
        tableLength: 100,
        tableData: [],
        updatePwdDlg: false,
        updateId: 0,
        p: 0,
        changeOptions: 0,
        chooseValue: [],
        options: [],//地区信息实时更新
        ruleForm: {
          //username: '',
          //password: '',
          //area: '',
          newPwd: '',
          oldPwd: ''
        },
        rules: {
          /*username: [
            {required: true, message: '请输入用户名', trigger: 'blur'},
            {min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '请输入密码', trigger: 'blur'},
            {min: 8, max: 18, message: '长度在 8 到 18 个字符', trigger: 'blur'}
          ],
          area: [
            {required: true, message: '请输入管理区域', trigger: 'blur'}
          ],*/
          pwd: [
            {required: true, message: '请输入密码', trigger: 'blur'},
            {min: 6, max: 16, message: '长度在 6 到 16 个字符', trigger: 'blur'}
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

  .areaChoose {
    width: 300px;
  }
</style>
