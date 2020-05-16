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
                        <input type="text" name="account" placeholder="用户名" id="account">
                        <i class="ui user icon"></i>
                      </div>
                    </div>
                    <div class="field">
                      <div class="ui left icon input">
                        <input type="password" name="password" placeholder="密码" id="password">
                        <i class="ui keyboard icon"></i>
                      </div>
                    </div>
                    <br>
                    <div class="ui teal fluid submit button" @click="onSubmit">登入</div>
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
    methods: {
      onSubmit() {
        $.fn.api.settings.api = {
          'login': 'http://101.37.173.57:8080/user/verification'
        };
        $('.ui.form').api({
          action: 'login',
          method: 'POST',
          on: 'now',
          beforeXHR: function (xhr) {
            xhr.setRequestHeader('content-type', 'application/json');
            return xhr;
          },
          data: JSON.stringify({
            "userId": $("#account").val(),
            "password": $("#password").val()
          }),
          //serializeForm: true,
          dataType: 'json',
          beforeSent: function (settings) {
            $('.ui.button').addClass("elastic loading");
            console.log(settings.data);
            return settings;
          },
          onSuccess: function (response, element, xhr) {
            $('.ui.button').removeClass("elastic loading");
            if (response.success) {
              //登录成功
              const store = window.sessionStorage;
              store.clear();//清空所有之前的数据
              console.log(response.UserId);
              store.setItem('user', response.UserId);
              $('body').toast({
                class: 'success',
                message: '登录成功！'
              });
              this.$router.push({path: '/dashboard'});
            }else{
              $('body').toast({
                  class: 'error',
                  message: '登录失败，原因：'+response.msg
                });
            }
          },
          onFailure: function (response, element, xhr) {
            $('.ui.button').removeClass("elastic loading");
            if (!response.success) {
              $('body').toast({
                class: 'error',
                message: '登录失败，原因：'+response.msg
              });
            }
          },
          onError: function (response, element, xhr) {
            $('.ui.button').removeClass("elastic loading");
            $('body').toast({
              class: 'error',
              message: '服务器请求错误！'
            });
          }
        });
      }
    },
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
