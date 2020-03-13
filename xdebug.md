我们在代码开发的时候，会使用 PhpStorm 结合 Xdebug 进行代码断点调试，这样能够追踪程序执行流程，方便调试代码，和发现潜在问题。

#### 原理
官方文档上给出了两种IDE与Xdebug的[交互过程以及相关配置](https://xdebug.org/docs/remote)：  
1、静态IP或者单人开发  

交互步骤：

![lWngqf](https://cdn.learnku.com/uploads/images/202001/09/21543/ylJRQL8ssK.gif!large)

官方解释：  
![lWnE2n](https://cdn.learnku.com/uploads/images/202001/09/21543/N2pg8Ge4ax.png!large)

配置`xdebug.remote_host`和`xdebug.remote_port`两个选项即可  
2、IP未知或者团队开发

![lWnkCj](https://cdn.learnku.com/uploads/images/202001/09/21543/9JY2OZUev9.png!large) 
配置`xdebug.remote_connect_back`和`xdebug.remote_port`两个选项即可  

#### 环境介绍

本人开发和调试环境为Vbox(virtualbox) + LNMP(linux、nginx、mysql、php)，IDE环境为：win7 + PhpStorm

#### Xdebug安装

[官方文档](https://xdebug.org/docs/install)针对不同环境提供了不同的安装方式，但比较推荐编译安装，因为不同的PHP版本安装的xdebug的版本也不尽相同，官方会针对你的PHP环境给出安装xdebug的版本并提供详细的安装步骤。  
![lWuAoD](https://cdn.learnku.com/uploads/images/202001/09/21543/jpGYHCenTA.png!large)
![lWu1w8](https://cdn.learnku.com/uploads/images/202001/09/21543/AeYd74x4pt.png!large)

按照给出的操作步骤进行操作：  
1、下载二进制文件压缩包，并上传到虚拟机,上传文件或者下载文件使用`rz`和`sz`指令，如果指令则安装`yum install lrzsz -y`    
2、安装扩展，`yum groupinstall "Development tools" && yum install php-devel autoconf automake`  
3、解压文件，`tar -xvzf xdebug-2.9.0.tgz`  
4、进入目录，`cd xdebug-2.9.0`  
5、执行`phpize`  
6、执行`./configure`  
7、执行`make && make install`  
8、执行`cp modules/xdebug.so /usr/lib64/php/modules`  
9、编辑`/etc/php.ini`,并加入一下内容  
```php
[Xdebug]
zend_extension = /usr/lib64/php/modules/xdebug.so
xdebug.remote_enable=1
xdebug.remote_autostart=1
xdebug.remote_host=192.168.1.64
xdebug.remote_port=9001
xdebug.remote_handler=dbgp
xdebug.idekey=PHPSTORM
```
10、重启php服务`service php-fpm restart`  

#### 配置PhpStorm
1、配置Xdebug端口
![lW1QIK](https://cdn.learnku.com/uploads/images/202001/09/21543/N23pOBu9ut.png!large)
2、配置DBGP代理
![lW18Re](https://cdn.learnku.com/uploads/images/202001/09/21543/qHtWDJWTFh.png!large)
3、配置server
![lW1NqI](https://cdn.learnku.com/uploads/images/202001/09/21543/SKlfTZKWNA.png!large)

#### 代码调试
![lW3YfU](https://cdn.learnku.com/uploads/images/202001/09/21543/AgGm0yPlsi.png!large)