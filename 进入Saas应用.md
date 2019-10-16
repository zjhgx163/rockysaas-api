# 1进入Saas应用(合作机构提供)

### 1. 接口调用场景&流程

##### 1.1 调用场景

用户在Saas商城我的应用里，点击

##### 1.2 流程图

![](<http://ydxjdnas.sanjinxia.com/image/%E8%BF%9B%E5%85%A5%E5%BA%94%E7%94%A8.jpg>)

##### 1.3  接口说明

如果此用户在合作机构是第一次购买，合作机构方需为此用户注册。

### 2. 请求说明

#### 2.1 接口调用方式

- post请求
- 支持https协议
- contentType ：text/json

#### 2.2  请求参数

| 参数        | 名称   | 值类型    | 是否必传 | 备注           |
| --------- | ---- | ------ | ---- | ------------ |
| platform  | 平台名  | string | Y    | 固定值rockysaas |
| userName  | 用户名  | string | Y    |              |
| secretKey | 密钥   | string | N    |              |

#### 2.3  响应参数

| 参数        | 名称            | 值类型    | 是否必传 | 备注                      |
| --------- | ------------- | ------ | ---- | ----------------------- |
| url       | 跳转地址          | string | Y    | 一般是登录后的首页               |
| loginType | 登录类型          | string | Y    | 1：session登录   2：token登录 |
| token     | 登录时的token     | string | N    | token和sessionId二选一      |
| sessionId | 登录时的sessionId | string | N    | token和sessionId二选一      |