# **设置推流中断超时时间**

## **1. 描述**

主播非正常结束推流时（因网络中断或者主播暂时休息等），播放端连接根据设置时间中断

## **2. 请求参数**

| **名称**   | **类型** | **是否必填** | **描述**                                                     |
| ---------- | -------- | ------------ | ------------------------------------------------------------ |
| username   | String   | 是           | 京东用户名pin                                                |
| signature  | String   | 是           | 用户签名，通过md5的方式校验用户的身份信息，保障信息安全。</br>md5=日期+username+秘钥SecretKey; 日期：格式为 yyyymmdd; username：京东用户名pin; 秘钥：双方约定; </br>示例：比如当前日期2016-10-23,用户pin:jcloud_00,用户秘钥SecretKey：e7a31b1c5ea0efa9aa2f29c6559f7d61,那签名为MD5(20161023jcloud_00e7a31b1c5ea0efa9aa2f29c6559f7d61)|
| domain     | String   | 是           | 加速域名|
|timeout | String   | 是           | 超时时间，单位是秒s，超时时间建议设置>1s，<=60s;极限值不超过120s |

## **3. 返回参数**

| **名称**   | **描述** | 
| ---------- | -------- |
| status  | 表示接口请求成功与否，成功用0表示，其他表示失败  | 
| msg  | 提示信息 | 
| data | 返回数据| 

## **4. 调用示例**

- ### **请求地址**

https://opencdn.jcloud.com/api/live/setPushStreamTimeout

- ### **请求示例**

```
{
    "username": "user_test",
    "signature": "ca4c56f85e3582f4d814cc77949c82a7",
    "domain":"test.jcloud.com",
    "timeout":"10"
}

```

- ### **返回示例**

```
{
    "status": 0,
    "msg": "",
    "data": {
        "PublishNormalTimeout": 10
    }
}
```
