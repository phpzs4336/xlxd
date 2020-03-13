>Composer 不是一个包管理器，是 PHP 用来管理依赖（dependency）关系的工具。你可以在自己的项目中声明所依赖的外部工具库（libraries），Composer 会帮你安装这些依赖的库文件。

### 安装
运行 Composer 需要 PHP 5.3.2+ 以上版本，composer 支持windows、linux等多平台。

#### linux上安装
1、执行php -v 查看PHP版本

![l1HWQA](https://s2.ax1x.com/2019/12/31/l1HWQA.png)

2、执行以下命令进行全局安装
```php
curl -sS https://getcomposer.org/installer | php
mv composer.phar /usr/local/bin/composer
```

![l1H6iD](https://s2.ax1x.com/2019/12/31/l1H6iD.png)

>`composer.phar` 是 Composer 的二进制文件。这是一个 PHAR 包（PHP 的归档），这是 PHP 的归档格式可以帮助用户在命令行中执行一些操作

3、查看是否安装成功
```php
//三选一
composer
composer -V
composer --version
```
![l1HHJg](https://s2.ax1x.com/2019/12/31/l1HHJg.png)

4、全局修改`composer`镜像
```php
//设置中国全量镜像，推荐使用阿里云镜像
composer config -g repo.packagist composer https://packagist.phpcomposer.com 
//查看配置是否成功
composer config -gl
```

![l1OY01](https://s2.ax1x.com/2019/12/31/l1OY01.png)

5、当某些不可抗因素导致原镜像不能正常使用的时候，切换composer镜像
```php
//1、切换源
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ 
//2、清除所有 package 缓存（此步奏选泽性操作）
composer clear-cache
//3、查看配置是否成功
composer config -gl
```
![l1XUDs](https://s2.ax1x.com/2019/12/31/l1XUDs.png)

6、如果需要解除镜像并恢复到 packagist 官方源
```php
composer config -g --unset repos.packagist
```

![l1vFSO](https://s2.ax1x.com/2019/12/31/l1vFSO.png)

7、composer升级 
```php
composer self-update
```

![l1Hxe0](https://s2.ax1x.com/2019/12/31/l1Hxe0.png)

中国全量镜像站内说如果版本更新不成功需要重新下载包。
![l1HOQs](https://s2.ax1x.com/2019/12/31/l1HOQs.png)

**注意：一般正常擦做到上述第4步就可以**

#### windos上安装composer

windows上安装composer比较方便，在[composer中文网](https://docs.phpcomposer.com/00-intro.html)直接下载exe文件进行一路确定安装即可，剩余的操作与linux上的操作无差别。

![l1zdQU](https://s2.ax1x.com/2019/12/31/l1zdQU.png)