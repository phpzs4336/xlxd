# 创建项目
>vue-cli3创建项目有两种方式,一种是通过命令行，一种是通过UI界面创建，在此我们使用命令行创建
>- 在指定项目根目录执行 `vue create -n my-project` 命令初始化项目,注意`-n`选项,如果不带`-n`,初始化项目的时候，也进行`git`初始化。
>- 系统提示两个选项进行选择：一是默认安装bable、eslint,二是自定义安装，我选择第二种
![init](https://s2.ax1x.com/2019/07/17/ZLfu1s.png)
>- 根据罗列出的几个选项和提示，我只选择安装bable,注意：选择选项的时候，按空格键是选择和不选择,按A键是全选，按I键是反选
>- 系统提示怎么设置配置文件，是单独配置还是放在package.json文件里，我选择单独配置
>- 最后一步提示，是否将这次创建项目的操作保留，等待以后创建项目的时候使用,我选择的是否
![init](https://s2.ax1x.com/2019/07/17/ZLf1BV.png)
>- 安装完成，执行`npm run serve` 命令，访问`http://localhost:8080/`
![init](https://s2.ax1x.com/2019/07/17/ZLfacR.png)

# 引入 element
> 在项目根目录执行`npm i element-ui -S`命令安装
> 在项目入口文件`main.js`引入`element`

```javascript
import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './App.vue';

Vue.use(ElementUI);

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
}).$mount('#app')
```
# 写一个小demo
```javascript
<template>
  <el-row type="flex" class="row-bg" justify="center">
    <el-col :span="6">
      <el-row>
        <el-button>账号登录</el-button>
        <el-button>免密登录</el-button>
      </el-row>
      <el-form label-width="80px" size="medium" class="login-form">
          <el-form-item label="邮箱">
            <el-input></el-input>
          </el-form-item>
          <el-form-item label="密码">
            <el-input></el-input>
          </el-form-item>
          <el-form-item label="验证码">
            <el-input class="login-input"></el-input>
            <div class="captcha" @click="getCaptcha">
              <span class="captcha-content" v-html="captchaTpl"></span>
            </div>
          </el-form-item>
        </el-form>
      <el-row>
        <el-button type="primary">提交</el-button>
        <el-button type="warning">重置</el-button>
      </el-row>
    </el-col>
  </el-row>
</template>

<script>
  import createCaptcha  from './../utils/createCaptcha';

  export default {
    data() {
      return {
        captchaTpl:'',
        originCaptcha:'',
      }
    },
    methods: {
      getCaptcha() {
        const captcha = createCaptcha();
        this.captchaTpl = captcha.tpl;
        this.originCaptcha = captcha.captcha;
      }
    },
    beforeMount() {
      this.getCaptcha();
    }
  }
</script>

<style>
  .login-form{
    margin-top: 20px;
  }
  .row-bg{
    margin: 200px auto;
  }
  .login-input{
    width:50%;
  }
  .captcha{
    cursor: pointer;
    width:50%;
    display: inline-block;
    background:#b73636;
    border-radius: 5px;
  }
  .captcha-content{
    font-size: 20px;
    color:white;
    display:inline-flex;
  }
  .captcha-content span{
    width:20px;
  }
  .login-btn{
    width:50%;
    height: 30px;
    line-height: 30px;
    border:1px solid #409EFF;
  }
</style>

```
# 结果展示
>不是专业前端，写的有点丑，只是假装会写前端，哈哈

![init](https://s2.ax1x.com/2019/07/17/ZLfTUS.png)
