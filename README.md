# wxcloudrun-wxcomponent-flask
[![GitHub license](https://img.shields.io/github/license/WeixinCloud/wxcloudrun-express)](https://github.com/WeixinCloud/wxcloudrun-express)
![GitHub package.json dependency version (prod)](https://img.shields.io/badge/python-3.7.3-green)

微信第三方平台微管家结合 python Flask 框架模版，由微管家接收消息，业务服务实现简单的计数器读写接口，使用云托管 MySQL 读写、记录计数值。

## 微管家说明
### 功能介绍
此项目提供第三方平台的后端服务以及第三方平台管理工具。该镜像可一键部署到微信云托管，分钟级别即可完成第三方平台开发环境搭建以及第三方平台管理工具部署。

##### 第三方平台推送消息
微信第三方平台需要填写两个URL用于接受官方推送的消息，详情参考官方文档：[创建与配置第三方平台准备工作](https://developers.weixin.qq.com/doc/oplatform/Third-party_Platforms/2.0/operation/thirdparty/prepare.html)。
- 授权事件URL: 本项目提供了接受官方推送并存入数据库的服务，对推送ticket、授权、解除授权的事件都做了相应处理。
- 消息与事件URL: 本项目提供了接受官方推送并存入数据库的服务，开发者可以读数据库查看推送消息，也可以在此基础上进行二次开发。
##### 第三方平台管理工具
- 授权帐号管理：可查看授权给第三方平台的公众号/小程序帐号信息。
- 第三方token获取：可一键获取component_verify_ticket、component_access_token、authorizer_access_token以及微信令牌，便于开发者进行调试。
- 第三方消息查看：可获取推送至授权事件URL和消息与事件URL的消息，便于开发者进行调试。
- 第三方授权页面生成：可一键生成PC版和H5版的授权页面，商家可扫码或者直接访问授权页面完成授权。


## Flask说明
### 目录结构说明

~~~
.
├── Dockerfile dockerfile       dockerfile
├── README.md README.md         README.md文件
├── container.config.json       微信云托管流水线配置
├── requirements.txt            依赖包文件
├── config.py                   项目的总配置文件  里面包含数据库 web应用 日志等各种配置
├── run.py                      flask项目管理文件 与项目进行交互的命令行工具集的入口
└── wxcloudrun                  app目录
    ├── __init__.py             python项目必带  模块化思想
    ├── dao.py                  数据库访问模块
    ├── model.py                数据库对应的模型
    ├── response.py             响应结构构造
    ├── templates               模版目录,包含主页index.html文件
    └── views.py                执行响应的代码所在模块  代码逻辑处理主要地点  项目大部分代码在此编写
~~~



### 服务 API 文档

#### `GET /api/count`

获取当前计数

##### 请求参数

无

##### 响应结果

- `code`：错误码
- `data`：当前计数值

###### 响应结果示例

```json
{
  "code": 0,
  "data": 42
}
```

##### 调用示例

```
curl https://<云托管服务域名>/api/count
```



#### `POST /api/count`

更新计数，自增或者清零

##### 请求参数

- `action`：`string` 类型，枚举值
  - 等于 `"inc"` 时，表示计数加一
  - 等于 `"clear"` 时，表示计数重置（清零）

###### 请求参数示例

```
{
  "action": "inc"
}
```

##### 响应结果

- `code`：错误码
- `data`：当前计数值

###### 响应结果示例

```json
{
  "code": 0,
  "data": 42
}
```

##### 调用示例

```
curl -X POST -H 'content-type: application/json' -d '{"action": "inc"}' https://<云托管服务域名>/api/count
```

## License

[MIT](./LICENSE)
