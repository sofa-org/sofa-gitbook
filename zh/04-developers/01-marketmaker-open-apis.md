# Open APIs for 做市商 

## 接口规范

### 请求约定

1. 为保证交易安全性，采用 HTTPS 传输
2. 使用 JSON 作为数据交互的格式
3. 统一采用 UTF-8 字符编码
4. 接口签名算法采用 HMAC-SHA256
5. 使用 UNIX 毫秒时间戳为自 1970 年 1 月 1 日 0 点 0 分 0 秒以来的毫秒数

### 接口参数

1. 接口请求参数固定为以下

| 参数名        | 类型   | 说明                                                   |
| ------------- | ------ | ------------------------------------------------------ |
| H-Request-Id  | string | [Header]请求 ID，要求不重复                            |
| H-Api-Key     | string | [Header]发起请求的 ApiKey                              |
| H-Timestamp   | long   | [Header]有效时间戳，如 1672387200000                   |
| H-Nonce       | string | [Header]随机字符串                                     |
| Authorization | string | [Header]认证类型 签名串，如：rfq-hmac-sha256 signature |

2. 接口响应参数固定为以下

| 参数名  | 类型    | 说明                            |
| ------- | ------- | ------------------------------- |
| code    | integer | [Body]错误码（详见附录-错误码） |
| message | string  | [Body]错误原因                  |
| value   | T       | [Body]结果                      |

### 签名生成

RFQ 服务平台要求合作方对请求进行签名。RFQ 服务平台会在收到请求后进行签名的验证。如果签名验证不通过，RFQ 服务平台将会拒绝处理请求，并返回 `401 Unauthorized`。

1. **构建签名串**

签名串一共有五行，每一行为一个参数。行尾以;结束，包括最后一行。其中，有效时间戳、请求随机串取自 head 里的 H-Timestamp 和 H-Nonce 参数。

```
有效时间戳;请求随机串;请求方法;URI;请求报文主体;
```

2. **计算签名值**

使用 SecretKey 对 `StringToSign` 进行 **HMAC-SHA256** 加密。

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

3. **设置 HTTP 头**

请求通过 `HTTP Authorization` 头来传递签名。`Authorization` 由**认证类型**和**签名信息**两个部分组成。

```
Authorization: 认证类型 签名信息
```

具体组成为：

1.认证类型，目前为 rfq-hmac-sha256

2.签名信息

- 签名值 `Signature`

```
Authorization: rfq-hmac-sha256 Signature
```

注意：

- 请求方法需要大写
- 用于签名的 `RequestBody` 需要和请求中的 Request Body 的内容保持一致
- 对于 GET, DELETE 请求，`URI` 需要包含请求的参数（如 /api/v1/result?orderId=123）。如果没有请求体（通常用于 GET 请求），则请求体使用空字符串""。
- 有效时间戳 H-Timestamp 由请求方确定，超过有效时间戳的请求将被 rfq 服务器拒绝。

## RFQ

### 提供 DNT RFQ 报价

```
GET rfq/dnt/quote
```

**入参**

| 参数名                            | 必填  | 类型   | 描述                                                                                                   | 内部注释             |
| --------------------------------- | ----- | ------ | ------------------------------------------------------------------------------------------------------ | -------------------- |
| vault                             | true  | string | 合约地址                                                                                               | 必要字段/唯一准确    |
| chainId                           | true  | int    | 链 ID                                                                                                  | 必要字段/唯一准确    |
| expiry                            | true  | long   | 到期日对应的秒级时间戳，例如 1672387200                                                                | 必要字段/唯一准确    |
| lowerBarrier                      | true  | number | 下方价格                                                                                               | 必要字段/唯一准确    |
| upperBarrier                      | true  | number | 上方价格                                                                                               | 必要字段/唯一准确    |
| depositAmount                     | true  | number | rfq 申购金额                                                                                           | 必要字段/唯一准确    |
| premiumAmount                     | true  | number | 申购 DNT 支付的权利金                                                                                  | 必要字段/唯一准确    |
| projectedFundingAmount            | false | number | 按照 rfq 提供的 Apy 预估的可以拿到的利息（RISKY 时为空）                                               | 必要字段/唯一准确    |
| deadline                          | true  | long   | rfq 过期时间对应的秒级时间戳，例如 1672387200                                                          | 必要字段/唯一准确    |
| takerWallet                       | false | string | 询价方钱包公共地址信息                                                                                 | 必要字段/唯一准确    |
| anchorPricesDecimal               | true  | long   | 转换为合约入参的倍数                                                                                   | 合约暗含[主要为便捷] |
| makerCollateralDecimal            | true  | long   | 转换为合约入参的倍数                                                                                   | 合约暗含[主要为便捷] |
| collateralAtRiskPercentageDecimal | true  | long   | 转换为合约入参的倍数                                                                                   | 合约暗含[主要为便捷] |
| totalCollateralDecimal            | true  | long   | 转换为合约入参的倍数                                                                                   | 合约暗含[主要为便捷] |
| underlyingPair                    | true  | string | 标的物交易对，例如 BTC-USDTUnderlying Pair, e.g. BTC-USDT                                              | 合约暗含[主要为便捷] |
| trackingSource                    | true  | string | 用于追踪交易对的数据来源，例如 DERIBITData sources used to track the underlying value, e.g. DERIBIT    | 合约暗含[主要为便捷] |
| depositCoin                       | true  | string | 申购 DNT 支付的权利金币种，与最终结算收益币种一致 Currency / Coin of the premium paid to subscribe DNT | 合约暗含[主要为便捷] |
| riskType                          | true  | string | 风险类型：PROTECTED, RISKY                                                                             |                      |

**出参**

| 目前字段名   | 提议字段名                 | 类型         | 描述                                           | 内部注释 |
| ------------ | -------------------------- | ------------ | ---------------------------------------------- | -------- |
| timestamp    |                            | long         | 报价对应的时间戳,秒级时间戳                    |          |
| vault        |                            | string       | 合约地址                                       | 充要信息 |
| chainId      |                            | int          | 链 ID                                          | 充要信息 |
| expiry       |                            | long         | 到期日对应的秒级时间戳，例如 1672387200        | 充要信息 |
| strikePrices | anchorPrices               | list[string] | 20000000000,30000000000                        | 充要信息 |
| premium      | makerCollateral            | string       | Maker 对赌抵押的金额                           | 充要信息 |
| amount       | totalCollateral            | string       | 总的质押的金额（Taker+Maker）0000000（E6,E18） | 充要信息 |
| payoff       | collateralAtRiskPercentage | string       | 保底时必填 E18                                 | 充要信息 |
| deadline     |                            | long         | 报价的过期时间秒级时间戳 例如 1672387200       | 充要信息 |
| makerWallet  |                            | string       | 签名的 Maker 方钱包地址                        | 充要信息 |
| signature    |                            | string       | 签名                                           | 充要信息 |

出参校验：

1. totalCollateral * collateralAtRiskPercentage - makerCollateral == 入参 premiumAmount
2. totalCollateral  - makerCollateral == 入参 depositAmount
3. 不相等/相差太多的话报价无效

**示例**

请求 URL&入参

```
rfq/dnt/quote
```

请求参数

```json
{
    **"rfqId"**:**1233992,**
    **"apy"**:**0.25,**
    **"tenor"**:**7.9,**
    **"fundingAmount"**:**0.01,**
    **"depositAmount"**:**1,**
    **"premiumCoin"**:**"BTC"**,
    **"premiumAmount":0.05,**
    **"bookingQuantity":1,**
    **"totalAmount":0.05,**
    **"payoff":1,**
    **"deadline":1672279892000,**
    **"signature":"dsdkksdsksdk"**
}
```

## 附录

| 错误码 | 错误信息                                                         |
| ------ | ---------------------------------------------------------------- |
| 1000   | system error.                                                    |
| 2001   | sign error.                                                      |
| 2002   | param error.                                                     |
| 3001   | Requested information does not exist.                            |
| 3002   | Deposit amount is outside _depositRange_.                        |
| 3003   | Maximum subscriptions limit is reached.                          |
| 3004   | Subscription failed due to too much difference in premiumAmount. |
| 3005   | Quote failed.                                                    |
| 3006   | Temporarily do not provide service.                              |
| 3007   | Api rate limit exceeded. Try slow down.                          |
| 3100   | Order creation failed.                                           |

**需要的函数**：

- premiumAmount = totalCollateral * dntCollateralRatio - makerCollateral
- depositAmount = totalCollater - makerCollateral
- bookingQuantity = totalCollateral * dntCollateralRatio
- projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365
- Tenor = (expDateTime - refDateTime) / 365
- APY-In-Range = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor
- APY-Protected = (projectedFundingAmount - premiumAmount) / tenor

