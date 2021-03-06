## 接口描述

本接口 (GetQueueAttributes) 用于获取某个已创建队列的属性。返回属性除了创建队列时设置的可设置属性外，还可以取到队列创建时间，最后一次修改队列属性时间，以及队列中消息的统计数据（近似值）。

**请参照下面说明将域名中的 {$region} 替换成相应地域：**
- 外网接口请求域名：`http(s)://cmq-queue-{$region}.api.qcloud.com`
- 内网接口请求域名：`http://cmq-queue-{$region}.api.tencentyun.com`


>**注意：**
>任何时候，包括内测期间，如果使用外网域名产生公网下行流量，都会收取流量费用。 所以强烈建议服务在腾讯云上的用户使用**内网**域名，内网不会产生流量费用。

**说明：**
- {$region}需用具体地域替换：gz（广州），sh（上海），bj（北京），shjr（上海金融），szjr（深圳金融），hk（香港），cd（成都），ca(北美)，usw（美西），sg（新加坡）。公共参数中的 region 值要与域名的 region 值保持一致，如果出现不一致的情况，以域名的 region 值为准，将请求发往域名 region 所指定的地域。
- 外网域名请求既支持 http，也支持 https。内网请求仅支持 http。
- 输入参数有些是可选的，不填取默认值。
- 输出参数在成功情况下所有出参都会返回给用户；失败情况下，至少会有 code, message, requestId 返回。

## 输入参数

以下请求参数列表仅列出了接口请求参数，其它参数见 [公共请求参数](https://cloud.tencent.com/document/product/295/7279) 页面。

| 参数名称 | 是否必选  | 类型 | 描述 |
|---------|---------|---------|---------|
| queueName| 是| String| 队列名字，在单个地域同一帐号下唯一。 队列名称是一个不超过 64 个字符的字符串，必须以字母为首字符，剩余部分可以包含字母、数字和横划线(-)。|


## 输出参数

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| code | Int | 0：表示成功，4440：队列不存在，其他返回值的含义可以参考 [错误码](/doc/api/431/5903)。|
| message | String | 错误提示信息。| 
| requestId| String| 服务器生成的请求 Id。出现服务器内部错误时，用户可提交此 Id 给后台定位问题。|
| maxMsgHeapNum| Int| 最大堆积消息数。取值范围在公测期间为 `1,000,000 - 10,000,000`，正式上线后范围可达到 `1000,000-1000,000,000`。默认取值在公测期间为 `10,000,000`，正式上线后为 `100,000,000`。|
| pollingWaitSeconds| Int| 消息接收长轮询等待时间。取值范围 0-30 秒，默认值 0。|
| visibilityTimeout| Int| 消息可见性超时。取值范围 1-43200 秒（即12小时内），默认值30。|
| maxMsgSize| Int| 消息最大长度。取值范围1024-65536 Byte（即1-64K），默认值65536。|
| msgRetentionSeconds| Int| 消息保留周期。取值范围 60-1296000秒（1min-15天），默认值 345600 秒 (4 天)。|
| createTime| Int| 队列的创建时间。返回 Unix 时间戳，精确到秒。|
| lastModifyTime| Int| 最后一次修改队列属性的时间。返回 Unix 时间戳，精确到秒。|
| activeMsgNum| Int| 在队列中处于 Active 状态（不处于被消费状态）的消息总数，为近似值。|
| inactiveMsgNum| Int| 在队列中处于 Inactive 状态（正处于被消费状态）的消息总数，为近似值。|
|rewindSeconds|Int | 回溯队列的消息回溯时间最大值，取值范围 0-43200 秒，0 表示不开启消息回溯。|
|rewindmsgNum|Int|已调用 DelMsg 接口删除，但还在回溯保留时间内的消息数量。|
|minMsgTime|Int|消息最小未消费时间，单位为秒。|

## 示例

输入：

<pre>
 https://domain/v2/index.php?Action=GetQueueAttributes
 &queueName=test-queue-123
 &<<a href="https://cloud.tencent.com/doc/api/229/6976">公共请求参数</a>>
</pre>

输出：

```
{
"code" : 0,
"message" : "",
"requestId":"14534664555",
"maxMsgHeapNum": 10000000,
"pollingWaitSeconds": 10,
"visibilityTimeout": 0,
"maxMsgSize": 65536,
"msgRetentionSeconds": 1296000,
"createTime":1462268960,
"lastModifyTime": 1462269960,
"activeMsgNum": 10000,
"inactiveMsgNum": 1000
}
```






