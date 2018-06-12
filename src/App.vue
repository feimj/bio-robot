<template>
  <div id="app">
    <div id="content">
      <StyleEditor ref="styleEditor" :code="currentStyle"></StyleEditor>
      <ResumeEditor ref="resumeEditor" :markdown="currentMarkdown" :enableHtml="enableHtml"></ResumeEditor>
    </div>
    <div id="foot">
      <ThankEditor ref="thankEditor" :markdown="currentThankMarkdown" :enableHtml="enableHtml"></ThankEditor>
    </div>
  </div>
</template>

<script>
  import StyleEditor from './components/StyleEditor'
  import ResumeEditor from './components/ResumeEditor'
  import ThankEditor from './components/ThankEditor'
  import './assets/reset.css'

  export default {
    name: 'app',
    components: {
      StyleEditor,
      ResumeEditor,
      ThankEditor
    },
    data() {
      return {
        interval: 5,
        currentStyle: '',
        enableHtml: false,
        fullStyle: [`
/**
 * Inspired by http://www.sitexa.org/anires/public/
 * Source code at https://github.com/sitexa/anires/
 * 您好，我是何敏健。
 * 现在，我用代码来写一份简历！
 */

/* 给所有元素加上过渡效果 */
* {
  transition: all .1s;
}

/* 设置背景颜色 */
html {
  color: rgb(222,222,222); background: rgb(0,64,64);
}
#content{
  height:70vh;
  margin:0;
}
#foot{
  height:29vh;
  margin:0;
}

/* 设置边框 */
.styleEditor {
  padding: .5em;
  border: 1px solid;
  margin: .5em;
  overflow: auto;
  width: 50vw; 
  height: 70vh;
  background: rgb(20,20,20);
}

/* 代码高亮 */
.token.selector{ color: rgb(130,150,0); }
.token.property{ color: rgb(190,140,0); }
.token.punctuation{ color: yellow; }
.token.function{ color: rgb(40,160,150); }

/* 加3D效果 */
html{
  perspective: 1000px;
}
.styleEditor {
  position: fixed; left: 0; top: 0;
  -webkit-transition: none;
  transition: none;
  -webkit-transform: rotateY(10deg) translateZ(-100px) ;
          transform: rotateY(10deg) translateZ(-100px) ;
}

/* 准备一个文本框，请看右边... */
.resumeEditor{
  position: fixed; right: 0; top: 30;
  padding: .5em;  margin: .5em;
  width: 50vw; height: 70vh;
  border: 1px solid;
  background: rgb(200,200,200); color: #222;
  overflow: auto;
  -webkit-transition: none;
  transition: none;
  -webkit-transform: rotateY(-10deg) translateZ(-100px) ;
          transform: rotateY(-10deg) translateZ(-100px) ;
}

/* 开始写简历，请看右边... */`, `

/**
 * 将Markdown格式翻译成HTML
 * 再对HTML加点样式
 */
.resumeEditor{
  padding: 2em;
}
.resumeEditor h2{
  display: inline-block;
  border-bottom: 1px solid;
  margin: 1em 0 .5em;
}
.resumeEditor ul,.resumeEditor ol{
  list-style: none;
}
.resumeEditor ul> li::before{
  content: '•';
  margin-right: .5em;
}
.resumeEditor ol {
  counter-reset: section;
}
.resumeEditor ol li::before {
  counter-increment: section;
  content: counters(section, ".") " ";
  margin-right: .5em;
}
.resumeEditor blockquote {
  margin: 1em;
  padding: .5em;
  background: #ddd;
}`, `

/* 写点感谢语，请看下方... */
.thankEditor{
  position: fixed;
  left: 1vw;
  bottom: 5vh;
  padding: .5em;  
  margin: .5em; 
  margin-top:.2em;
  font-size: .8em;
  width: 97vw; 
  height: 20vh;
  border: 1px solid #ccc;
  background: rgb(10,10,10);
  color: rgb(0,200,0);
  overflow: auto;
}
.thankEditor ul,.thankEditor ol{
  list-style: none;
}
.thankEditor ul> li::before{
  content: '☞'; 
  color: red;
  margin-right: .5em;
}
.thankEditor ol {
  counter-reset: section;
}
.thankEditor ol li::before {
  counter-increment: section;
  content: counters(section, "☞") " ";
  margin-right: .5em;
}`],
        currentMarkdown: '',
        currentThankMarkdown: '',
        fullMarkdown: `
何敏健
====

广东广州

Java软件工程师 物联网项目经理 现为自由职业

技能
====

数据库设计
----
  - 结构化数据库设计

后端开发
----
  - 用户管理
  - 单点登录
  - 权限管理
  - 设备管理
  - 空间管理
  - 实时监控
  - 历史管理
  - 生产管理
  - 加工管理
  - 质检管理
  - 物流管理
  - 远程控制

前端开发
----
  - Web前端开发

项目经验
----
  - 2017/03 - 2018/04 智慧农业大棚
  - 2016/08 - 2017/01 智能交通沙盘
  - 2016/06 - 2016/08 智慧农业生产过程追溯系统
  - 2016/04 - 2016/06 智能环境监测系统
  - 2015/07 - 2016/03 智能家居控制系统

技术及语言
----
  - Java: Struts2, SpringMVC, Tiles, Spring, SpringData JPA, QueryDSL, Hibernate, Mina, POI, ITEXT, Flyway, CAS
  - DB: MySQL, SQLServer
  - WebServer: Nginx, Tomcat
  - OS: Windows, Ubuntu
  - UI: Bootstrap, EasyUI, HIGHCHARTS, ECHARTS, Font Awesome, jQuery Validation, jQuery Mockjax 
  - Others: SVN, GIT, Powershell

工作经历
----

1. 2015/05 - 2018/06 广州飞瑞敖电子科技股份有限公司 
   Java软件工程师兼项目经理

教育经历
----

1. 2011/09 - 2015/06 华南农业大学 信息与计算科学 学士

联系方式
----

* 微信：FEI_MJ
* QQ：fei_mj
* 手机：15017528321
* 邮箱：15017528321@163.com`, 
        thanksMarkdown: `感谢耐心观看！
----

* 此项目的创意及源代码均非原创，仅出于好奇进行非商业性质的修改，如有侵权请多多包涵，谢谢。
* 何敏健 2018.05.24`
      }
    },
    created() {
      this.makeResume()
    },

    methods: {
      makeResume: async function () {
        await this.progressivelyShowStyle(0);
        await this.progressivelyShowResume();
        await this.progressivelyShowStyle(1);
        await this.showHtml();
        await this.progressivelyShowStyle(2);
        await this.progressivelyShowThanks()
      },
      showHtml() {
        return new Promise((resolve, reject) => {
          this.enableHtml = true
          resolve()
        })
      },
      progressivelyShowStyle(n) {
        return new Promise((resolve, reject) => {
          let interval = this.interval
          let showStyle = (async function () {
            let style = this.fullStyle[n]
            if (!style) {
              return
            }
            // 计算前 n 个 style 的字符总数
            let length = this.fullStyle.filter((_, index) => index <= n).map((item) => item.length).reduce((p, c) => p + c, 0)
            let prefixLength = length - style.length
            if (this.currentStyle.length < length) {
              let l = this.currentStyle.length - prefixLength
              let char = style.substring(l, l + 1) || ' '
              this.currentStyle += char
              if (style.substring(l - 1, l) === '\n' && this.$refs.styleEditor) {
                this.$nextTick(() => {
                  this.$refs.styleEditor.goBottom()
                })
              }
              setTimeout(showStyle, interval)
            } else {
              resolve()
            }
          }).bind(this)
          showStyle()
        })
      },
      progressivelyShowResume() {
        return new Promise((resolve, reject) => {
          let length = this.fullMarkdown.length
          let interval = this.interval
          let showResume = () => {
            if (this.currentMarkdown.length < length) {
              this.currentMarkdown = this.fullMarkdown.substring(0, this.currentMarkdown.length + 1)
              let lastChar = this.currentMarkdown[this.currentMarkdown.length - 1]
              let prevChar = this.currentMarkdown[this.currentMarkdown.length - 2]
              if (prevChar === '\n' && this.$refs.resumeEditor) {
                this.$nextTick(() => this.$refs.resumeEditor.goBottom())
              }
              setTimeout(showResume, interval)
            } else {
              resolve()
            }
          }
          showResume()
        })
      },
      progressivelyShowThanks() {
        return new Promise((resolve, reject) => {
          let length = this.thanksMarkdown.length
          let interval = this.interval
          let showThanks = () => {
            if (this.currentThankMarkdown.length < length) {
              this.currentThankMarkdown = this.thanksMarkdown.substring(0, this.currentThankMarkdown.length + 1)
              let lastChar = this.currentThankMarkdown[this.currentThankMarkdown.length - 1]
              let prevChar = this.currentThankMarkdown[this.currentThankMarkdown.length - 2]
              if (prevChar === '\n' && this.$refs.thankEditor) {
                this.$nextTick(() => this.$refs.thankEditor.goBottom())
              }
              setTimeout(showThanks, interval)
            } else {
              resolve()
            }
          }
          showThanks()
        })
      }
    }
  }

</script>

<style scoped>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  html {
    min-height: 100vh;
  }

  * {
    box-sizing: border-box;
  }
</style>
