# 修改配置信息

在第一步完成之后，进入到了 `gonelist_linux_amd64` 文件夹，在 `config.json` 文件中有许多可以配置的地方，建议填入自己的 `client_id` 以及 `client_secret`，`folder_sub` 可以设置 GONEList 的根目录是从 onedrive 的什么地方开始

```
{
  //------建议填入自己的 id 和 secret --------
  "client_id": "16e320f7-e427-4612-88da-f3d03e944d40",
  "client_secret": "lURpL3U@bBlmJ0:_dnU.LeLOGNGdVT30",
  "redirect_url": "http://localhost:8000/auth",
  // 设置一个自己喜欢的字符串
  "state": "23333",
  // token 的路径，推荐默认
  "token_path": "",
  // 下载链接重定向前缀
  "download_redirect_prefix": "",
  "server": {
    // 监听的端口
    "port": 8000,
    // 自动刷新的时间单位是分钟，默认 10 分钟，不要超过 1 小时
    "refresh_time": 10,
    // 登陆成功后，跳转的 URL，可不设置，新版已自动跳转
    "site_url": "http://localhost:8000",
    // 自定义 onedrive 的子文件夹
    "folder_sub": "/",
    //静态页面的目录，默认当前路径下的dist目录
    "dist_path": "./dist/",
    // 是否绑定到 0.0.0.0
    "bind_global": true
  },
  // 世纪互联设置
  "china_cloud": {
    "enable": false,
    "client_id": "2b54b127-b403-42a3-8b55-d25f3119aa13",
    "client_secret": "a0CGqBT3f_8U5gztxKjxR-LNW-ZnTe.m"
  },
  // 给文件夹设置密码，相比此方法，更加建议直接在文件夹下的创建 .password 设置密码
  "pass_list": [
    {
      "path": "",
      "pass": ""
    }
  ]
}
```