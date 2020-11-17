# API

记录接口和对应的功能

# 返回格式

暂时对于正常的返回使用的 http code 都是 200，如果出现 503 等错误是服务器内部错误

```json
{
  "code": 200,
  "msg": "ok",
  "data": []
}
```

具体错误根据返回内容中的 code 判断，对应的 code 和错误对照表

# 登陆部分

- `GET /login`
  登陆接口，访问时判断是否已经登录

  - 如果已经登录，跳转到 `/onedrive/getallfiles` - 如果没有登陆，跳转到 `/loginmg`

- GET `/loginmg`
  跳转到微软登陆验证页面

- GET `/auth`
  用来接收 code，接收后会请求 AccessToken，初始化自动刷新

- GET `/onedrive/getallfiles`
  获取所有文件的树结构，返回 data 字段结构如下

```json
{
  "name": "macOS Catalina",
  "path": "/macOS Catalina",
  "is_folder": true,
  "last_modify_time": "2020-03-04T06:51:34Z",
  "children": [
    {
      "name": "Dark Mode JPEG_Original.jpeg",
      "path": "/macOS Catalina/Dark Mode JPEG_Original.jpeg",
      "is_folder": false,
      "last_modify_time": "2020-03-04T06:52:27Z",
      "children": null
    }
  ]
}
```

## 获取某个文件夹内容 getpath

- GET `/onedrive/getpath?path=xxx`
  通过相对路径获取文件列表，和 `/onedrive/getallfiles` 返回一样
  文件夹可以通过 `download_url`是否有值来判断是否需要访问 README 接口
  需要 pass，[密码验证](#密码验证)

如果第一次登陆，还未加载完成，返回 600，文件未加载完成

接到返回内容如下

```json
{
  "code": 200,
  "msg": "ok",
  "data": {
    "name": "exampleType",
    "path": "/exampleType",
    "readme_url": "",
    "is_folder": true,
    "download_url": "",
    "last_modify_time": "2020-04-27T09:46:11Z",
    "size": 2736654191,
    "children": [
      {
        "name": "image",
        "path": "/exampleType/image",
        "readme_url": "",
        "is_folder": true,
        "download_url": "",
        "last_modify_time": "2020-04-27T09:46:59Z",
        "size": 6729069,
        "children": null
      },
      {
        "name": "song",
        "path": "/exampleType/song",
        "readme_url": "",
        "is_folder": true,
        "download_url": "",
        "last_modify_time": "2020-04-30T05:27:39Z",
        "size": 19090445,
        "children": null
      },
      {
        "name": "txt",
        "path": "/exampleType/txt",
        "readme_url": "",
        "is_folder": true,
        "download_url": "",
        "last_modify_time": "2020-04-24T14:29:14Z",
        "size": 12,
        "children": null
      },
      {
        "name": "video",
        "path": "/exampleType/video",
        "readme_url": "",
        "is_folder": true,
        "download_url": "",
        "last_modify_time": "2020-04-30T05:27:43Z",
        "size": 2710834487,
        "children": null
      },
      {
        "name": "README.md",
        "path": "/exampleType/README.md",
        "readme_url": "",
        "is_folder": false,
        "download_url": "https://gonelist-my.sharepoint.com/personal/xuan_gonelist_onmicrosoft_com/_layouts/15/download.aspx?UniqueId=7c3b08a1-db5b-48a5-b91b-be33b53c4b78&Translate=false&tempauth=eyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0.eyJhdWQiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAvZ29uZWxpc3QtbXkuc2hhcmVwb2ludC5jb21AZjhmNTY3YWMtZjY1OC00ZWM0LWE5ZTEtZmZhODQ5ZDFjM2M3IiwiaXNzIjoiMDAwMDAwMDMtMDAwMC0wZmYxLWNlMDAtMDAwMDAwMDAwMDAwIiwibmJmIjoiMTU5NDY5ODExNCIsImV4cCI6IjE1OTQ3MDE3MTQiLCJlbmRwb2ludHVybCI6IkpKMnJrc3d5WXB0L0FSdmo4d3dkK1pSejN4SHF4N2dDLzk3RzRiZ3BxbVk9IiwiZW5kcG9pbnR1cmxMZW5ndGgiOiIxNjEiLCJpc2xvb3BiYWNrIjoiVHJ1ZSIsImNpZCI6IlpHTmlOVGcwTmprdFlqUmtZUzAwWWprM0xXRTVNREV0T0RZeE4ySmpNelV3WmpneCIsInZlciI6Imhhc2hlZHByb29mdG9rZW4iLCJzaXRlaWQiOiJNV1E0WWpFelltWXRNekpsWlMwMFlUY3hMVGd4WVRZdE1tRTFOelV6WXpZMk5UZzMiLCJhcHBfZGlzcGxheW5hbWUiOiJnb25lbGlzdC5jdWd4dWFuLmNuIiwiZ2l2ZW5fbmFtZSI6Inh1YW4iLCJmYW1pbHlfbmFtZSI6ImN1ZyIsImFwcGlkIjoiYzA3MGE0NGMtZjI1ZC00NWRmLTkwMTctN2E1MDY5MzYzZjk0IiwidGlkIjoiZjhmNTY3YWMtZjY1OC00ZWM0LWE5ZTEtZmZhODQ5ZDFjM2M3IiwidXBuIjoieHVhbkBnb25lbGlzdC5vbm1pY3Jvc29mdC5jb20iLCJwdWlkIjoiMTAwMzIwMDBBRjBFNjI4MyIsImNhY2hla2V5IjoiMGguZnxtZW1iZXJzaGlwfDEwMDMyMDAwYWYwZTYyODNAbGl2ZS5jb20iLCJzY3AiOiJteWZpbGVzLnJlYWQiLCJ0dCI6IjIiLCJ1c2VQZXJzaXN0ZW50Q29va2llIjpudWxsfQ.YlovR0ZsLzNEcXFUWktvdksxQzNvS0l4dXVuazBEY1J3Tnd6bVcwL3F5az0&ApiVersion=2.0",
        "last_modify_time": "2020-07-13T14:53:47Z",
        "size": 178,
        "children": null
      }
    ]
  }
}
```

## 获取 README

- GET `/README?path=xxx`
  获取 README 的内容，如果改目录有密码，同样无法获取，获取示例如下
  需要 pass，[密码验证](#密码验证)

```json
{
  "code": 200,
  "msg": "ok",
  "data": "<h1>exampleType</h1>\n\n<p>这里是一些测试的文件夹</p>\n\n<p>你可以在每个文件夹里面定义对应的 README.md 文件</p>\n\n<p>按照 markdown 语法编写即可，可以直接进行浏览</p>\n"
}
```

## 下载文件

- GET `/d/*path`
  获取 path 的内容，如果要下载 xxx/.password，返回

```
{
    "code": 10008,
    "msg": ".password 文件禁止下载",
    "data": ".password 文件禁止下载"
}
```

否则会重定向到下载链接

## 全局搜索功能

- GET `/onedrive/search?key=xxx`
  输入关键字进行搜索

如果没有找到，则返回 data 空数组

```
{
    "code": 200,
    "msg": "ok",
    "data": [
        "/exampleType/test-pass/test-second",
        "/exampleType/test.password-123456",
        "/exampleType/test-pass",
        "/exampleType/test"
    ]
}
```

如果内部索引为空或者出错，返回

```
{
  "code": 10002,
  "msg": "未找到对应项目",
  "data": ""
}
```

# 密码验证

如果使用接口获取的文件夹设置了密码，通过 header 里面 pass 来传值，访问格式如下

```http
GET /onedrive/getpath?path=/exampleType HTTP/1.1
Host: gonelist.cugxuan.cn
pass: 123456
```

如果密码错误，返回

```json
{
  "code": 10007,
  "msg": "输入密码错误",
  "data": "输入密码错误"
}
```
