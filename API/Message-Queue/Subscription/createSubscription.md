# createSubscription


## 描述
创建订阅

## 请求方式
POST

## 请求地址
https://jcq.jdcloud-api.com/v1/regions/{regionId}/topics/{topicName}/subscriptions

|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**regionId**|String|True| |所在区域的Region ID|
|**topicName**|String|True| |topic 名称|

## 请求参数
|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**consumerGroupId**|String|True| |consumerGroupId|


## 返回参数
|名称|类型|描述|
|---|---|---|
|**requestId**|String| |


## 返回码
|返回码|描述|
|---|---|
|**200**|OK|
