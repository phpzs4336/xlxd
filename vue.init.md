>使用cli3创建的项目很简洁，如果你想使用webpack模板创建项目，或者习惯用`vue init`的方式创建项目怎么办？不用担心，cli3有提供插件可以使用cli3版本之前的方式创建项目，下面演就演示一下

- 执行`vue init --help`它会提示你首先安装`@vue/cli-init`这个插件

![vue](https://s2.ax1x.com/2019/07/18/ZXqhSP.png)

- 执行`vue install -g @vue/cli-init`命令

![vue](https://s2.ax1x.com/2019/07/18/ZXqLYn.png)
- 执行`vue init webpack my-project`命令就可以创建`webpack`模板的项目了

![vue](https://s2.ax1x.com/2019/07/18/ZXLkf1.png)


>扩展：模板分类有如下几个
『simple』：通过在一个 HTML 文件中引入 Vue 来使用它；
『browserify』 和 『webpack』： 分别使用 browserify 和 webpack 作为打包工具，并配置了对应的 Vue 组件转换工具和热加载等功能；
『pwa』(Progressive Web App)：一种基于前面的『webpack』的模板；