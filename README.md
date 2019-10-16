# msg接入说明

### 1. 接入平台流程

1. 申请appId
2. 获取平台公钥，提交机构公钥

### 2. API通用说明

#### 2.1 数据交互规范

- 使用POST请求

- 使用HTTPS协议

- 通过RSA加解密算法完成数据签名验证

- 请求和响应均采用json形式，数据均为utf8编码

- 接口保持幂等性：重复请求请返回相同的结果

  ​

#### 2.2 服务商调用平台接口参数

##### 2.2.1 请求参数

| 参数名称      | 类型     | 是否必传 | 说明                               |
| --------- | ------ | ---- | -------------------------------- |
| appId     | string | Y    | 唯一商户id                           |
| method    | string | Y    | 请求的方法                            |
| bizParams | string | Y    | 请求的业务数据，此处数据格式为Json封装。具体参数查看详细接口 |
| timestamp | long   | Y    | 10位时间戳，精确到秒                      |
| sign      | string | Y    | API请求的签名                         |

##### 2.2.2 响应参数

| 参数名称    | **名称** | 类型           | 是否必传 | 说明                  |
| ------- | ------ | ------------ | ---- | ------------------- |
| success | 处理成功标志 | boolean      | Y    | 接口处理是否成功            |
| code    | 响应返回码  | int          | Y    | 200表示成功，非200为异常情况   |
| msg     | 响应信息   | string       | Y    | 返回失败原因等，success标识成功 |
| data    | 返回业务数据 | json(object) | N    | 具体参数查看详细接口          |

 

#### 2.3 平台调用服务商接口参数

##### 2.3.1 请求参数

| 参数名称      | 类型     | 是否必传 | 说明                               |
| --------- | ------ | ---- | -------------------------------- |
| bizParams | string | Y    | 请求的业务数据，此处数据格式为Json封装。具体参数查看详细接口 |
| sign      | string | Y    | API请求的签名                         |
| timestamp | long   | Y    | 10位时间戳，精确到秒                      |

##### 2.3.2 响应参数

| 参数名称    | 名称     | 类型           | 是否必传 | 说明                  |
| ------- | ------ | ------------ | ---- | ------------------- |
| success | 处理成功标志 | boolean      | Y    | 接口处理是否成功            |
| code    | 响应返回码  | int          | Y    | 200表示成功，非200为异常情况   |
| msg     | 响应信息   | string       | Y    | 返回失败原因等，success标识成功 |
| data    | 返回业务数据 | json(object) | N    | 具体参数查看详细接口          |

