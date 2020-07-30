# 多用户

当前单应用不支持多用户，可以进行多应用部署，思路如图

![multi](./img/multi-account/multi.png ':size=70%')

在下载之后一个压缩文件，解压两个文件夹

```
// 首先按照之前的方法下载好压缩包
$ ls
gonelist_linux_amd64.tar.gz
// 解压两次，第一次为 gonelist1，第二次为 gonelist2
$ tar -zxf gonelist_linux_amd64.tar.gz && mv gonelist_linux_amd64 gonelist1
$ tar -zxf gonelist_linux_amd64.tar.gz && mv gonelist_linux_amd64 gonelist2

// 解压完成后你的目录结构应该是这样的
$ ls
gonelist1  gonelist2  gonelist_linux_amd64.tar.gz
// 展示一下目录结构
$ tree -L 2
.
├── gonelist1
│   ├── config.json
│   ├── dist
│   └── gonelist_linux_amd64
├── gonelist2
│   ├── config.json
│   ├── dist
│   └── gonelist_linux_amd64
└── gonelist_linux_amd64.tar.gz
```

然后修改对应 config.json，注意对应端口（port）应该修改得不一样
然后按照正常的流程启动两个应用，并且登陆即可
**建议浏览器使用无痕模式进行登录，否则第二个应用很可能登陆的是同样的账号**

完成之后即可通过不同端口访问，如果需要进行反向代理，可以看 [反向代理和 HTTPS](reverse-https)