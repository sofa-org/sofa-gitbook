
# 市场做市商的开放API

## 规范

### 前缀

1. 为确保交易安全，使用HTTPS进行传输。
2. JSON是数据交换格式。
3. 普遍采用UTF-8字符编码。
4. 接口签名算法使用HMAC-SHA256。
5. 使用UNIX毫秒时间戳，表示自1970年1月1日0:00:00以来的毫秒数。

### 参数

#### 请求

| 名称          | 类型   | 备注                                                  |
| ------------- | ------ | ----------------------------------------------------- |
| H-Request-Id  | string | [Header]请求ID，唯一                                  |
| H-Api-Key     | string | [Header]API密钥                                       |
| H-Timestamp   | long   | [Header]有效时间戳，例如：1672387200000               |
| H-Nonce       | string | [Header]随机字符串                                    |
| Authorization | string | [Header]签名，例如：[mm_id]-hmac-sha256签名           |

#### 响应

| 名称    | 类型    | 备注             |
| ------- | ------- | ---------------- |
| code    | integer | [Body]错误代码   |
| message | string  | [Body]错误原因   |
| value   | T       | [Body]结果       |

### 签名生成

我们的RFQ平台要求合作伙伴签名以批准请求，随后进行验证程序。验证失败将导致平台拒绝并返回`401`未授权响应。

#### 构建签名字符串：

签名字符串由五行组成，每行代表一个参数。每行以分号结束，包括最后一行。有效时间戳和请求随机数分别取自头部的`H-Timestamp`和`H-Nonce`参数。

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### 填写签名

使用 `SecretKey` 通过 `HMAC-SHA256` 加密 `StringToSign`。

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### 设置 HTTP 头

请求通过 `HTTP Authorization` 头传递签名。`Authorization header` 由两个部分组成：**认证类型** 和 **签名信息**。

```
Authorization: AuthenticationType SignatureInformation
```

1. 认证类型: `[mm_id]-hmac-sha256`
2. 签名信息: `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_注意:_

- 请求方法应为大写。
- 用于签名的 `RequestBody` 必须与请求主体的内容匹配。
- 对于 GET 和 DELETE 请求，`URI` 应包括请求参数（例如，/api/v1/result?orderId=123）。
  - 如果没有请求主体（GET 请求中常见），请求主体应为空字符串（""）。
- 有效时间戳（H-Timestamp）由请求者确定；超出有效时间戳的请求将被 RFQ 服务器拒绝。

## RFQ

### 提供 DNT RFQ 报价

```
GET rfq/dnt/quote
```

**参数**

| 名称                    | 必需   | 类型   | 描述                                                                 |
| ----------------------- | ------ | ------ | -------------------------------------------------------------------- |
| vault                   | 是     | string | Vault 地址                                                          |
| chainId                 | 是     | int    | 链 ID                                                               |
| expiry                  | 是     | long   | 过期时间，以秒为单位的时间戳，例如：1672387200                      |
| lowerBarrier            | 是     | number | 下限                                                                |
| upperBarrier            | 是     | number | 上限                                                                |
| depositAmount           | 是     | number | 存款                                                                |
| premiumAmount           | 是     | number | 保费                                                                |
| protectedFundingAmount  | 否     | number | 受保护的利息（RISKY 时为 null）                                     |
| deadline                | 是     | long   | 报价截止时间，例如：1672387200                                      |
| takerWallet             | 否     | string | 承接方钱包地址                                                      |
| anchorPricesDecimal     | 是     | long   |                                                                    |
| makerCollateralDecimal  | 是     | long   |                                                                    |
| collateralAtRiskDecimal | 是     | long   |                                                                    |
| totalCollateralDecimal  | 是     | long   |                                                                    |
| underlyingPair          | 是     | string | 标的对，例如：BTC-USDT                                              |
| trackingSource          | 是     | string | 用于跟踪标的价值的数据来源，例如：DERIBIT                           |
| depositCoin             | 是     | string | 支付 DNT 订阅的保费的货币/币种                                      |
| tradingFeeRate          | 是     | number |                                                                    |
| settlementFeeRate       | 是     | number |                                                                    |
| riskType                | 是     | string | 类型：PROTECTED, RISKY                                              |

**响应**

| 名称                       | 类型         | 描述                                 |
| -------------------------- | ------------ | ------------------------------------ |
| timestamp                  | long         | 报价时间戳                           |
| vault                      | string       |                                      |
| chainId                    | int          |                                      |
| expiry                     | long         | 过期时间戳，例如：1672387200         |
| anchorPrices               | list[string] | 20000000000,30000000000              |
| makerCollateral            | string       |                                      |
| totalCollateral            | string       |                                      |
| collateralAtRisk           | string       | E18                                |
| makerBalanceThreshold      | string       |                                    |
| deadline                   | long         | 报价截止时间戳                       |
| makerWallet                | string       |                                    |
| signature                  | string       | .                                   |

注意：

1. $$collateralAtRisk - makerCollateral ==  premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**示例**

请求 URL

```
rfq/dnt/quote
```

参数

```json
{
    "rfqId":1233992,
    "apy":0.25,
    "tenor":7.9,
    "fundingAmount":0.01,
    "depositAmount":1,
    "premiumCoin":"BTC",
    "premiumAmount":0.05,
    "bookingQuantity":1,
    "totalAmount":0.05,
    "payoff":1,
    "deadline":1672279892000,
    "signature":"dsdkksdsksdk"
}
```
### 提供牛市/熊市趋势 RFQ 报价

```

GET rfq/smart-trend/quote
```

**参数**

| 名称                    | 必需   | 类型   | 描述                                                               |
| ----------------------- | ------ | ------ | ----------------------------------------------------------------- |
| vault                   | 是     | string | 保管库地址                                                         |
| chainId                 | 是     | int    | 链 ID                                                             |
| expiry                  | 是     | long   | 到期时间，以秒为单位的时间戳，例如：1672387200                    |
| direction               | 是     | string | 看涨 / 看跌                                                       |
| lowerStrike             | 是     | number | 下限                                                              |
| upperStrike             | 是     | number | 上限                                                              |
| depositAmount           | 是     | number | 存款                                                              |
| premiumAmount           | 是     | number | 保费                                                              |
| protectedFundingAmount  | 否     | number | 保护的利息（RISKY 时为 null）                                     |
| deadline                | 是     | long   | 报价截止时间，例如：1672387200                                    |
| takerWallet             | 否     | string | 承接者钱包地址                                                   |
| anchorPricesDecimal     | 是     | long   |                                                                   |
| makerCollateralDecimal  | 是     | long   |                                                                   |
| collateralAtRiskDecimal | 是     | long   |                                                                   |
| totalCollateralDecimal  | 是     | long   |                                                                   |
| underlyingPair          | 是     | string | 标的对，例如：BTC-USDT                                            |
| trackingSource          | 是     | string | 用于跟踪标的价值的数据来源，例如：DERIBIT                         |
| tradingFeeRate          | 是     | number |                                                                   |
| settlementFeeRate       | 是     | number |                                                                   |
| depositCoin             | 是     | string | 支付以订阅 Smart Trend 的保费的货币/币种                          |
| riskType                | 是     | string | 类型：PROTECTED, RISKY                                            |

**响应**

| 名称             | 类型         | 描述                                 |
| ---------------- | ------------ | ------------------------------------ |
| timestamp        | long         | 报价时间戳                           |
| vault            | string       |                                      |
| chainId          | int          |                                      |
| expiry           | long         | 到期时间戳，例如：1672387200         |
| anchorPrices     | list[string] | 20000000000,30000000000              |
| makerCollateral  | string       |                                      |
| totalCollateral  | string       |                                      |


| collateralAtRisk | string       | E18                                |
| deadline         | long         | 报价截止时间戳                      |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

注意：

- $$PremiumAmount = BookingQuantity * UnitQuotePrice$$
- $$MaxPayoutAmount = BookingQuantity * (upperStrike - lowerStrike)$$
- $$MinAPYAmount == projectedFundingAmount - premiumAmount$$
- $$MaxAPYAmount == MinAPYAmount + MaxPayoutAmount$$
- $$makerCollateral = MaxPayoutAmount - premiumAmount$$
- $$collateralAtRisk = MaxPayoutAmount$$
- $$totalCollateral = depositAmount + makerCollateral$$
- $$anchorPrices = [lowerStrike, upperStrike]$$

### 提供双重 RFQ 报价

```
GET rfq/dual/quote
```

**参数**

| 名称                    | 必需   | 类型   | 描述                                                             |
| ----------------------- | ------ | ------ | ---------------------------------------------------------------- |
| vault                   | true   | string | 保管库地址                                                       |
| chainId                 | true   | int    | 链 ID                                                            |
| expiry                  | true   | long   | 到期时间，以秒为单位的时间戳，例如：1672387200                   |
| strike                  | true   | number | 行权价                                                           |
| type                    | true   | string | CALL 或 PUT                                                      |
| depositAmount           | true   | number | 存款                                                             |
| deadline                | true   | long   | 报价截止时间，例如：1672387200                                   |
| refDateTime             | true   | long   | 当前请求时间                                                     |
| takerWallet             | false  | string | 接受者钱包地址                                                   |
| anchorPriceDecimal      | true   | long   |                                                                 |
| makerCollateralDecimal  | true   | long   |                                                                 |
| totalCollateralDecimal  | true   | long   |                                                                 |
| underlyingPair          | true   | string | 标的资产对，例如：BTC-USDT                                       |
| trackingSource          | true   | string | 用于跟踪标的价值的数据来源，例如：DERIBIT                        |
| depositCoin             | true     | string | 支付订阅双币投资的货币/币种             |
| depositCoinTokenAddress | true     | string |                                       |
| depositCoinTokenDecimal | true     | long   |                                       |
| tradingFeeRate          | true     | number | .                                     |

**Response**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | 报价时间戳                         |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | 到期时间戳，例如：1672387200       |
| anchorPrice      | string       | 20000000000                        |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| deadline         | long         | 报价截止时间戳                     |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

## 附录

| Code | Message                                                          |
| ---- | ---------------------------------------------------------------- |
| 1000 | 系统错误。                                                       |
| 2001 | 签名错误。                                                       |
| 2002 | 参数错误。                                                       |
| 3001 | 请求的信息不存在。                                               |
| 3002 | 存款金额超出_depositRange_。                                     |
| 3003 | 达到最大订阅限制。                                               |
| 3004 | 由于premiumAmount差异过大，订阅失败。                           |
| 3005 | 报价失败。                                                       |
| 3006 | 暂时不提供服务。                                                 |
| 3007 | 超出API速率限制。请放慢速度。                                   |
| 3100 | 订单创建失败。                                                   |

**必需的功能**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
```

- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$
