<template>
  <div>
    <div class="ui top fixed borderless massive menu">
      <div class="item">
        <i class="teal th list icon"></i>
        <strong>仪表盘</strong>
      </div>
      <a class="item" href="/dashboard"> 主页 </a>
      <a class="item"> 账号管理 </a>
      <div class="right menu">
        <a class="item">账号设置
          <div class="ui teal label"> {用户名}</div>
        </a>
      </div>
    </div>
    <div class="box"></div>
    <div class="box"></div>
    <div class="box"></div>
    <div class="box"></div>
    <div class="ui container">
      <div class="box"></div>
      <div class="ui raised very padded segment" style="border-color: #f9f9ff">
        <h1 class="bt">通知</h1>
        <div class="ui divider"></div>
        <div class="box"></div>
        <div class="ui divided items">
          <div class="item">
            <div class="content">
              <a class="header">第一条通知</a>
              <div class="meta">
                <span>通知日期</span>
              </div>
              <div class="description">
                <p>通知的详细内容</p>
                <p></p>
              </div>
              <div class="extra">
                备注信息
              </div>
            </div>
          </div>
          <div class="item">
            <div class="content">
              <a class="header">第二条通知</a>
              <div class="meta">
                <span>通知日期</span>
              </div>
              <div class="description">
                <p>通知的详细内容</p>
                <p></p>
              </div>
              <div class="extra">
                备注信息
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="box"></div>
      <div class="ui raised very padded segment" style="border-color: #f9f9ff">
        <h1 class="bt">今日</h1>
        <div class="ui divider"></div>
        <div class="box"></div>
        <div class="ui stackable three column grid">
          <div class="column">
            <div class="ui raised very padded segment">
              <div class="box"></div>
              <h1 class="center aligned ui header">N/A 件<br>
                <div class="sub header">总活动</div>
              </h1>
              <div class="box"></div>
            </div>
          </div>
          <div class="column">
            <div class="ui raised very padded segment">
              <div class="box"></div>
              <h1 class="center aligned ui header">N/A 件<br>
                <div class="sub header">申请中活动</div>
              </h1>
              <div class="box"></div>
            </div>
          </div>
          <div class="column">
            <div class="ui raised very padded segment">
              <div class="box"></div>
              <h1 class="center aligned ui center header">N/A 件<br>
                <div class="center sub header">进行中活动</div>
              </h1>
              <div class="box"></div>
            </div>
          </div>
        </div>
        <div class="box"></div>
      </div>
      <div class="box"></div>
      <div class="ui raised very padded segment" style="border-color: #f9f9ff">
        <h1 class="bt">功能</h1>
        <div class="ui divider"></div>
        <div class="box"></div>
        <div class="ui stackable three column grid">
          <div class="column">
            <a class="ui link raised card" href="/list">
              <div class="content">
                <div class="box"></div>
                <h2 class="center aligned header">活动管理</h2>
                <div class="box"></div>
                <div class="center aligned description">点击本卡片，您将进入管理活动页面。该页面内您可通过限制搜索条件导出任何活动数据。</div>
                <div class="box"></div>
              </div>
            </a>
          </div>
          <div class="column">
            <a class="ui link raised card" href="/new">
              <div class="content">
                <div class="box"></div>
                <h2 class="center aligned header">新增活动</h2>
                <div class="box"></div>
                <div class="center aligned description">点击本卡片，您将进入管理活动页面。该页面内您可通过限制搜索条件导出任何活动数据。</div>
                <div class="box"></div>
              </div>
            </a>
          </div>
          <div class="column">
            <a class="ui link raised card" href="/users">
              <div class="content">
                <div class="box"></div>
                <h2 class="center aligned header">人员导出</h2>
                <div class="box"></div>
                <div class="center aligned description">点击本卡片，您将进入管理活动页面。该页面内您可通过限制搜索条件导出任何活动数据。</div>
                <div class="box"></div>
              </div>
            </a>
          </div>
        </div>
      </div>
      <div class="box"></div>
      <div class="ui center aligned very padded segment" style="background-color: #f9f9ff;border-color: #f9f9ff">
        <div class="ui inline">
          <h1 class="ui teal massive header">
            需要帮助?
          </h1>
        </div>
        <h3 class="ui medium header" style="color:darkgrey">请联系网站管理人员，或查阅相关提示</h3>
        <div class="box"></div>
        <div class="large ui teal basic button" @click="gettoast()"><strong>获取提示</strong></div>
      </div>
      <div class="box"></div>
      <div class="ui divider"></div>
      <div class="center aligned">
        <div class="ui center aligned big header" style="color:lightgrey"><strong>
          <div style="color:#2ecc72;display:inline-block">X</div>
          Poi
        </strong></div>
      </div>
      <div class="box"></div>
    </div>
  </div>
</template>

<script>
  export default {
    methods: {
      gettoast() {
        $('body').toast({
          title: '抱歉，暂时无法处理您的请求',
          message: '由于文档仍在建设，我们暂时不能回应您的请求。',
          showProgress: 'bottom'
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
        title: '仪表盘'
      }
    },
  }
</script>

<style scoped>
  body {
    background-color: #fafafa;
  }

  .box {
    height: 10px;
  }

  .bt {
    color: #269093;
  }
</style>
