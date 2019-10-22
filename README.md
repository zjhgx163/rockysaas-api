# 接入说明

### 1. 接入平台流程

1. 申请appId

2. 获取平台公钥，提交机构公钥。平台公钥为

   ```bash
   MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCr3rUEE4PpowG0bdPHBb4D27n7XNK3B+TSsneXDwf5rECnAmwRVnWQSSi8U7fccKpeVJt0+CYs/Q6Dcxa1VutJyR1yMeSNwyd66scpF4rNsXv4FEGvuZOgpD3MBESvmJxEFCgwPgkuiSDGXq2/2sIhgmtOyTWx9QDSFAGMrN9VtwIDAQAB
   ```


### 2. API通用说明

#### 2.1 数据交互规范

- 使用POST请求

- 使用HTTPS协议

- 通过RSA加解密算法完成数据签名验证

- 请求和响应均采用json形式，数据均为utf8编码

- 接口保持幂等性：重复请求请返回相同的结果


#### 2.2 服务商调用平台接口参数

##### 2.2.1 请求参数

| 参数名称      | 类型     | 是否必传 | 说明                               |
| --------- | ------ | ---- | -------------------------------- |
| appId     | string | Y    | 唯一商户id                           |
| method    | string | Y    | 请求的方法                            |
| bizParams | string | Y    | 请求的业务数据，此处数据格式为Json封装。具体参数查看详细接口 |
| timestamp | long   | Y    | 13位时间戳，精确到毫秒                     |
| sign      | string | Y    | API请求的签名（不参与签名）                  |

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
| sign      | string | Y    | API请求的签名（不参与签名）                  |
| timestamp | long   | Y    | 10位时间戳，精确到秒                      |

##### 2.3.2 响应参数

| 参数名称    | 名称     | 类型           | 是否必传 | 说明                  |
| ------- | ------ | ------------ | ---- | ------------------- |
| success | 处理成功标志 | boolean      | Y    | 接口处理是否成功            |
| code    | 响应返回码  | int          | Y    | 200表示成功，非200为异常情况   |
| msg     | 响应信息   | string       | Y    | 返回失败原因等，success标识成功 |
| data    | 返回业务数据 | json(object) | N    | 具体参数查看详细接口          |

### 3. 签名加密规则

为保证数据传输过程不被篡改，所有接口需要签名和验签，签名算法为RSA，接收参数时需要验签，验签失败拒绝请求，不处理任何逻辑。

#### 3.1 服务商调用平台签名请求发送过程

1. 获取平台提供的appId（如："SA0001"）

2. 构造业务参数bizParams（业务参数为jsonString作为整体参数加签）和基础参数并对其按key值按ASCII进行排序（sign不参与签名）：例如服务商用户初始化结果通知参数排序后如下

   ```Java
   {
   	"appId":"SA0001",
    	"bizParams":"{\"orderNo\":\"726723761214065669\", \"secretKey \":\"secret\",\"userName\":\"test\"}",
    	"method":"api.saas.v1.user.init-result-notify",
    	"timestamp":"1571650367181"
   }
   ```

3. 按照参数的顺序拼接参数（key0=value0&key1=value1...），如

   ```Java
   appId=SA0001&bizParams={"orderNo":"726723761214065669","secretKey":"secret","userName":"test"}&method=api.saas.v1.user.init-result-notify&timestamp=1571650367181
   ```

4. 使用服务商的私钥生成签名（算法：MD5withRSA）

5. post请求发送，json格式

   ```Java
   {
    	"appId":"1234",
    	"bizParams":"{\"orderNo\":\"726723761214065669\"
    		,\"secretKey\":\"secret6\",\"userName\":\"test7\"}",
    	"method":"api.saas.v1.user.init-result-notify",	 	
    	"sign":"xxxxxxxxxxxxx",
    	"timestamp":"1571651366370"
   }
   ```

6. 接受返回参数并处理逻辑


#### 3.2 平台调用服务商签名请求发送过程

1.  签名算法和3.1一致

2. 参照2.3参数进行参数排序，排序方式和3.1一致

   ```Java
   {
   		"bizParams":"{\"orderNo\":\"726723761214065669\"}",
   		"timestamp":"1571650367181"
   }
   ```

3.  按照参数的顺序拼接参数（key0=value0&key1=value1...）

    ```Java
    bizParams={"orderNo":"726723761214065669"}&timestamp=1571650367181
    ```

4.  使用平台私钥生成签名（算法：MD5withRSA）

5.  post请求发送，json格式

    ```Java
        {
        	"bizParams":"{\"orderNo\":\"726723761214065669\"}",
            "sign":"xxxxxxxxxxxxxxxxx",
         	"timestamp":"1571651366370"
        }
    ```

    ​

6.  服务商使用平台提供的公钥进行验签

### 4. 代码示例

[]: 

