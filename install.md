# 使用 GONEList 搭建在线分享网盘

[GONEList](https://github.com/cugxuan/gonelist) 是由 `golang+vue` 编写的基于 onedrive 的在线网盘，有着跨平台等特性，搭建简单。本次搭建使用的是系统是 `64 位 Ubuntu 18.04`，Linux 系统一般都可以兼容对应的操作

{% note warning %}

- [下载已编译的程序](#下载已编译的程序)
- [创建 MicroSoft 应用](#创建-MicroSoft-应用)
- [修改配置信息](#修改配置信息)
- [启动和终止应用](#启动和终止应用)
- [HTTPS](#HTTPS)

{% endnote %}

# 下载已编译的程序

在 [Github-Release](https://github.com/cugxuan/gonelist/releases) 或者 [gonelist-release](https://gonelist.cugxuan.cn/#/gonelist-release) 中对应的包，Linux 系统下载 gonelist_linux_amd64.tar.gz，直接运行即可启动，以 Linux 系统本地启动为例

```
// 下载对应的安装包，也可下载 gonelist-release 中的包，下面命令不一定是最新版本
$ wget https://github.com/cugxuan/gonelist/releases/download/v0.4.1/gonelist_linux_amd64.tar.gz
// 解压进入文件夹
$ tar -zxf gonelist_linux_amd64.tar.gz && cd gonelist_linux_amd64/
```

如果你是更新，那么请先删除原本的压缩包

# 创建 MicroSoft 应用

如果您的 **整个网盘以及微软账号** 里面没有任何隐私资料，可以直接使用我们的默认 `client_id` 和 `client_secret`（解压后 `config.json` 自带默认设置），直接跳转到 [修改配置信息](#修改配置信息)，否则请申请自己单独的应用来**保护您的隐私安全**。
（注：您可以通过 [你已授予访问权限的应用和服务](https://account.live.com/consent/Manage) 来管理自己许可的应用）

## 注册应用程序

打开 [注册应用程序](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) 的链接 ( 世纪互联版的[点击这里](https://portal.azure.cn/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade) )，登陆后选择「注册应用程序」，输入「名称」，选择「任何组织目录中的账户和个人」（**注意这里不要看位置选择而是看文字，部分人可能是中间那个选项，不要选成单一租户或者其他选项，否则会导致登陆时出现问题**），输入重定向 URL 为 `http://localhost:8000/auth`，「注册」即可

![register-app](./img/install/register-app.png)

## 客户端密码

注册好应用程序之后，选择「证书和密码」，点击「新客户端密码」，输入一串密码，选择时间为「永久」，点击「添加」
（注：在添加之后输入的密码之后会消失，**请记录下来 client_secret 的值**）

![client_secret](./img/install/client_secret.png)

## API 权限

选择「API 权限」，点击 「MicroSoft Graph」，在「选择权限」中输入 `file`，勾选 「Files.read」（注：`Files.read` 是只读最小权限，图中权限较大，也同样可以），点击「确定」

![api-auth](./img/install/api-auth.png)

## 客户端 id

在 config 中还有一项是客户端 id，在概述中记录下即可，至此我们的 `client_id` 和 `client_secret` 都已经拿到了

![client_id](./img/install/client_id.png)

# 修改配置信息

关于 config.json 的详细信息，请看 [参数配置](parameter)

# 启动和终止应用

现在就可以直接启动应用了

```
// 启动应用，建议先启动，成功部署之后使用后台运行
// 在命令行 ctrl+c 可以停止运行
$ ./gonelist_linux_amd64
// 后台运行应用
$ nohup ./gonelist_linux_amd64 >nohup.log 2>&1 &
// 结束后台应用，首先需要找出进程号，输出里面的第二列
$ ps -ef | grep go
root     21020 20947  0 20:08 pts/3    00:00:00 ./gonelist_linux_amd64
// kill + 进程号停止 gonelist 进程
$ kill 21020
```

浏览器打开 `http://localhost:8000` 即可访问，如果你是部署在服务器，可以使用 `http://yoursite:8000` 访问。打开后会跳转登陆页面，授予权限

![allow-auth](./img/install/allow-auth.png)

如果是在本地部署，登陆成功会跳转到首页，此时已经完成部署。如果登陆后一直没有反应，可能是因为文件夹数量过多导致，建议设置「子文件夹」选项
如果是在服务器部署，登陆成功会跳转到`http://localhost:8000/auth?code=xxx`，将当前网址改成 `http://yoursite:8000/auth?code=xxx` 再回车等待文件加载后，会自动跳转你的网站 `http://yoursite:8000`

# 反向代理和 HTTPS

请看[反向代理和HTTPS](reverse-https)
