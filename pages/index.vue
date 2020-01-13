<template>
  <div class="full">
    <el-row type="flex" justify="center" align="middle">
      <el-col :span="7">
        <div class="grid-content">
          <div class="ui container">
            <div class="box"></div>
            <div>
              <div class="ui center aligned segments msg">
                <div class="ui teal inverted raised segment" style="color:white;text-align: center">
                  <h4><i class="ui user icon"></i>管理员登入</h4>
                </div>
                <div class="ui centered raised very padded segment">
                  <form class="ui form">
                    <div class="field">
                      <div class="ui left icon input">
                        <input type="text" name="username" placeholder="用户名">
                        <i class="ui user icon"></i>
                      </div>
                    </div>
                    <div class="field">
                      <div class="ui left icon input">
                        <input type="password" name="password" placeholder="密码">
                        <i class="ui keyboard icon"></i>
                      </div>
                    </div>
                    <br>
                    <button class="ui teal fluid button" type="submit">登入</button>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  export default {
    methods: {},
    head() {
      return {
        script: [
          {src: 'https://cdn.jsdelivr.net/npm/jquery@3.3.1/dist/jquery.min.js'},
          {src: 'https://cdn.jsdelivr.net/npm/fomantic-ui@2.8.2/dist/semantic.min.js'}
        ],
        link: [
          {
            rel: 'stylesheet',
            type: 'text/css',
            href: 'https://cdn.jsdelivr.net/npm/fomantic-ui@2.8.2/dist/semantic.min.css'
          }
        ],
        title: '管理端登入'
      }
    },
  }
</script>

<style scoped>
  * {
    margin: 0;
    padding: 0;
  }

  .grid-content {
    background-color: #f9f9ff;
    border-radius: 4px;
    margin-top: 30%;
  }
</style>
