# 进入Saas应用(合作机构提供)

### 1. 接口调用场景&流程

##### 1.1 调用场景

用户在Saas商城我的应用里，点击进入相应应用主页

##### 1.2 流程图

![](http://ydxjdnas.sanjinxia.com/image/进入应用.jpg)

##### 1.3  接口说明

进入应用时浏览器直接跳转到服务商提供的跳转页面，然后服务商接收到请求后需要执行登录操作

### 2. 请求说明

#### 2.1 接口调用方式

- 跳转时用get请求
- 支持https协议

#### 2.2  请求参数

| 参数      | 名称 | 值类型 | 是否必传 | 备注                             |
| :-------------- | :----- | :-------- | :--- | :----------------- |
| platform  | 平台名 | string | Y    | 固定值rockysaas                                             |
| userName  | 用户名 | string | Y        | 25个字符内                                                  |
| secretKey | 密钥   | string | N        | 建议服务商为每个用户生成一个密钥，以增加安全性，100个字符内 |

#### 2.3  响应参数

无

#### 2.4 请求示例

##### GET 请求（注意bizParams为字符串）：

```Java
 http://wddev.panshi101.com/warranty.jsp?appId=x&bizParams=x&sign=x&timestamp=xxxx
```

