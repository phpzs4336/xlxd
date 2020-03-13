### 背景
>我们在开发或者维护项目的时候，由于历史的原因，不通项目用的`node`或者`npm`版本不一致，这无疑给我们开发维护增加了困难。为了解决这个困难，Node Version Manager 应运而生，简称`nvm`,它是 node 版本管理，方便在你的电脑或者虚拟机上安装不同版本的node/npm进行开发和维护，很好的降低了成本。


### 准备工作
>如果在此之前你安装了node的话，请彻底卸载相关内容，如果没有安装过则忽略此准备工作

* 执行 npm cache clean --force 命令清除缓存目录中的所有缓存文件
* 电脑卸载程序卸载node
* 找到以下目录进行查找相关node/npm的内容，一旦找到就删除掉相关目录
    - C:\Program Files (x86)\Nodejs
    - C:\Program Files\Nodejs
    - C:\Users\{User}\AppData\Roaming\npm (或者 %appdata%\npm)
    - C:\Users\{User}\AppData\Roaming\npm-cache (或者 %appdata%\npm-cache)
    - C:\Users\{User}\.npmrc(或者查询带`.`前缀的文件)
    - C:\Users\{User}\AppData\Local\Temp\npm-*
* 删除有关`node`和`npm`的环境变量(用户变量和系统变量)
* 重启电脑


### 安装步骤
>nvm的安装是分mac/linux/windows的，在此我仅演示windows版本，[GitHub地址](https://github.com/coreybutler/nvm-windows)

- 下载NVM安装包 [戳我下载](https://github.com/coreybutler/nvm-windows/releases)
![nvm](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zMi5heDF4LmNvbS8yMDE5LzA3LzE3L1pxUnY5QS5wbmc)
>下面对罗列的不同资源讲解一下,我下载的是安装包
>- nvm-noinstall.zip： 这个是绿色免安装版本，但是使用之前需要配置
>- nvm-setup.zip：这是一个安装包，下载之后点击安装，无需配置就可以使用，方便。
>- Source code(zip)：zip压缩的源码
>- Sourc code(tar.gz)：tar.gz的源码，一般用于*nix系统

- 傻瓜式安装(一路狂戳，安装完成)
>安装完成后，输入`nvm version`命令，如显示你安装的版本，则安装成功
![nvm](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zMi5heDF4LmNvbS8yMDE5LzA3LzE3L1pxV2VjcS5wbmc)
- 配置
    - 修改node镜像地址：`nvm node_mirror https://npm.taobao.org/mirrors/node/`
    - 修改npm镜像地址：`nvm node_mirror https://npm.taobao.org/mirrors/npm/`


 ### 使用
>- nvm list [available]                 罗列出已安装的或者官方提供可下载的node版本
>- nvm install <version\>                下载指定版本的node,如果为latest,则表示下载最新版本
>- nvm use <version\>                    使用或者切换node版本
>- nvm uninstall <version\>              卸载已安装的node版本
>- nvm version                           查看当前nvm版本
>- nvm node_mirror <node_mirror_url\>    设置node镜像地址
>- nvm npm_mirror <npm_mirror_url\>      设置npm镜像地址
![nvm](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9zMi5heDF4LmNvbS8yMDE5LzA3LzE3L1pxZmlLeC5wbmc)



























