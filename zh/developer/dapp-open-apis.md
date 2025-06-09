
# DApp 开放 API

我们在 sofa.org 上现有的 dApp 是一个关键的工作流程功能，也是用户与 SOFA 协议交互的主要方式。然而，我们鼓励其他开发者通过他们自己的 dApp 访问和连接 SOFA，以最大化我们的生态系统增长。我们将这些开发者视为我们的“经纪人”合作伙伴，并鼓励有兴趣的各方联系 SOFA 团队以获取更多 API 信息。

## DNT

### 推荐的 DNT RFQ 列表查询

```
GET /rfq/dnt/recommended-list
```

**输入参数**

| **字段名称**       | **必需**  | **类型** | **描述**                                                                                           |
| ------------------ | --------- | -------- | -------------------------------------------------------------------------------------------------- |
| vault              | true      | string   | 合约信息                                                                                           |
| chainId            | true      | int      | 链 ID                                                                                              |

**响应参数**

| **字段名称** | **类型**     | **描述**                                      |
| ------------ | ------------ | --------------------------------------------- |
| code         | int          | 0 表示返回结果正常                            |
| message      | string       | 异常情况下返回的错误信息                      |
| value        | list[object] | 如下所示                                      |

对象

| **字段名称**               | **类型**     | **描述**                                                                                             |
| -------------------------- | ------------ | ---------------------------------------------------------------------------------------------------- |
| rfqId                      | number       | RFQ ID                                                                                               |
| chainId                    | int          | 链 ID                                                                                                 |
| vault                      | string       | 合约地址                                                                                              |
| riskType                   | string       | 风险类型：PROTECTED, RISKY                                                                           |
| forCcy                     | string       | 标的货币                                                                                              |
| domCcy                     | string       | 货币对                                                                                                |
| depositCcy                 | string       | 认购货币                                                                                              |
| lowerBarrier               | number       | 下限价格                                                                                              |
| upperBarrier               | number       | 上限价格                                                                                              |
| depositAmount                | number       | RFQ 购买金额                                                                                     |
| expiry                       | number       | 到期时间戳（例如：1672387200）                                                                  |
| timestamp                    | number       | 当前定价的触发时间；下一个观察开始时间基于此逻辑计算                                           |
| observationStart             | number       | 基于时间戳估算的敲入/敲出观察开始时间                                                           |
| feeRate                      | object       | 交易和结算费率（可选）                                                                          |
| leverageInfo                 | object       | 贷款信息（可选）                                                                                |
| relevantDollarPrices         | list[object] | RCH 价格转换和空投计算所需的代币价格（可选）                                                    |
| amounts                      | object       | 计算金额（可选）                                                                                |
| apyInfo                      | object       | 年化信息，适用于非 Surge 产品（可选）                                                           |
| oddsInfo                     | object       | 几率信息，适用于 Surge 产品（可选）                                                             |
| quote                        | object       |                                                                                                 |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                        |
| \> makerCollateral           | string       | 制造商的抵押金额                                                                                |
| \> totalCollateral           | string       | 总抵押金额（接受者+制造商）                                                                     |
| \> collateralAtRisk          | string       | 保证时所需（可选）                                                                              |
| \> makerBalanceThreshold     | string       | 制造商余额阈值                                                                                  |
| \> deadline                  | number       | 到期时间戳（例如：1672387200）                                                                  |
| \> makerWallet               | string       | 制造商的钱包（可选）                                                                            |
| \> signature                 | string       | 签名（可选）                                                                                    |

**请求示例**

```
GET rfq/dnt/recommended-list?vault=xxxxxx&chainId=1
```

**响应**

```json
{
    "code":0,
    "message":"",
    "value":[
        {
            "rfqId":1233992,
            "riskType":"PROTECTED",
            "forCcy":"BTC",
            "expiry":1672387200,
            "lowerBarrier":18000,
            "upperBarrier":24000,
```

```json
            "depositCcy":"BTC",
            "depositAmount": 0.05,
            "protectedApy":0.01,
            "deadline":1672279892,
            "recommended":true,
            "quote":
                {
                    "rfqId":1233992,
                    "inRangeApy":0.25,
                    "tenor":7.9,
                    "fundingAmount":0.25,
                    "depositAmount":1,
                    "premiumCoin":"BTC",
                    "premiumAmount":0.05,
                    "bookingQuantity":0.3,
                    "totalAmount":0.05,
                    "payoff":0.3,
                    "deadline":1672279892,
                    "signature":"dsdkksdsksdk"
                }
            
        }
    ]
}
```

### DNT 询价

- 注意事项:
  - 请不要在纯询价请求中传递用户的钱包地址。
  - 只有在订阅时才应传递用户的钱包地址。

```
GET /rfq/dnt/quote
```

**输入参数**

| **字段名称**       | **必需**     | **类型** | **描述**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | 合约信息                                                                                                   |
| chainId            | true         | int      | 链ID                                                                                                               |
| expiry             | true         | number     | 到期日期的秒级时间戳，例如：1672387200                                                           |
| lowerBarrier       | true         | number   | 下限价格                                                                                                            |
| upperBarrier       | true         | number   | 上限价格                                                                                                            |
| depositAmount      | true         | number   | RFQ购买金额                                                                                                    |
| inputApyDefinition | true         | string   | 底层代码是枚举，指示输入APY的计算方式：OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | 保证年收益率（RISKY为空，protected必填）                                                      |
| fundingApy         | false        | number   | AAVE年收益率（RISKY为空，protected必填）                                                            |
| takerWallet        | false        | string   | 询问者的钱包公钥地址信息                                                                           |

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0表示返回结果正常   |
| message        | string       | 异常情况下返回的错误信息 |
| value          | object       | 如下所示                                 |

对象

| **字段名称**               | **类型**     | **描述**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | 合约地址                                                                                  |
| chainId                      | int          | 链ID                                                                                          |
| riskType                     | string       | 风险类型：PROTECTED, RISKY                                                                       |
| forCcy                       | string       | 标的货币                                                                               |
| domCcy                       | string       | 货币对                                                                                     |
| depositCcy                   | string       | 认购货币                                                                             |
| lowerBarrier                 | number       | 下限价格                                                                                       |
| upperBarrier                 | number       | 上限价格                                                                                       |
| depositAmount                | number       | RFQ购买金额                                                                               |
| expiry                       | number         | 到期时间戳（例如：1672387200）                                                               |
| timestamp                    | number         | 当前定价的触发时间；下次观察开始时间基于此逻辑计算 |
| observationStart             | number         | 基于时间戳估算的敲入/敲出观察开始时间                        |
| feeRate             | object         | 交易和结算费率（可选）                        |
| leverageInfo             | object         | 贷款信息（可选）                        |
| relevantDollarPrices             | list[object]         | RCH价格转换和空投计算所需的代币价格（可选）                       |
| amounts             | object         | 计算出的金额（可选）                       |
| apyInfo             | object         | 年化信息，适用于非Surge产品（可选）                     |
| oddsInfo             | object         | 概率信息，适用于Surge产品（可选）                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Maker的抵押金额                                                                         |
| \> totalCollateral            | string       | 总抵押金额（Taker+Maker）                                                             |
| \> collateralAtRisk | string       | 在保证时需要（可选）                                                           |
| \> makerBalanceThreshold | string       | Maker余额阈值                                                          |
| \> deadline                   | number         | 到期时间戳（例如，1672387200）                                                           |
| \> makerWallet                | string       | Maker的钱包（可选）                                                                     |
| \> signature                  | string       | 签名（可选）                                                                          |

### DNT获胜概率

在到期前保持在界限内的概率

```
GET rfq/dnt/winning-probabilities
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | 标的货币      |
| expiry          | true         | number     | 到期日期的秒级时间戳，例如，1672387200 |
| lowerBarrier       | true         | number   | 下限价格 |
| upperBarrier       | true         | number   | 上限价格 |

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0表示返回结果正常   |
| message        | string       | 异常情况下返回的错误信息 |
| value          | list[object] | 如下所示                                 |

对象


| **字段名称**   | **类型**     | **描述**                        |
| -------------- | ------------ | --------------------------------|
| spotPrice      | number       | 现货价格                        |
| timestamp      | number       |                                |
| probabilities  | object       | 获胜概率                        |

**请求示例**

```
GET rfq/dnt/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerBarrier=xxx&upperBarrier=xxx
```

**响应**

```json
{
    "code":0,
    "message":"",
    "value": {
        "spotPrice": 62121,
        "timestamp": 1727080594,
        "probabilities": {
            "probDntStayInRange": 0.4,
            "probBullTrendItmLowerStrike": null,
            "probBullTrendItmUpperStrike": null,
            "probBearTrendItmLowerStrike": null,
            "probBearTrendItmUpperStrike": null,
        }
    }
}
```

## 智能趋势

### 推荐智能趋势 RFQ 列表查询

```
GET rfq/smart-trend/recommended-list
```
**输入参数**

| **字段名称**       | **必需**  | **类型** | **描述**                                         |
| -------------------- | ------------- | -------- | ------------------------------------------------------------ |
| vault              | true          | string   | 合约信息                               |
| chainId            | true          | int      | 链ID                    |

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0表示返回结果正常   |
| message        | string       | 异常情况下返回的错误信息 |
| value          | list[object] | 如下所示                                 |

对象

| **字段名称**               | **类型**     | **描述**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | 链ID                                                                                          |
| vault                        | string       | 合约地址                                                                                  |
| riskType                     | string       | 风险类型：PROTECTED, RISKY                                                                       |
| direction | string | BULLISH,BEARISH |   
| forCcy                       | string       | 标的货币                                                                               |
| domCcy                       | string       | 货币对                                                                                     |
| depositCcy                   | string       | 认购货币                                                                             |
| lowerBarrier                 | number       | 下限价格                                                                                       |
| upperBarrier                 | number       | 上限价格                                                                                       |
| depositAmount                | number       | RFQ购买金额                                                                               |
| expiry                       | number         | 到期时间戳（例如：1672387200）                                                               |
| timestamp                    | number         | 当前定价的触发时间；下次观察开始时间基于此逻辑计算 |
| feeRate             | object         | 交易和结算费率（可选）                        |
| leverageInfo             | object         | 贷款信息（可选）                        |
| relevantDollarPrices             | list[object]        | RCH价格转换和空投计算所需的代币价格（可选）                       |
| amounts             | object         | 计算金额（可选）                       |
| apyInfo             | object         | 年化信息，适用于非Surge产品（可选）                     |
| oddsInfo             | object         | 赔率信息，适用于Surge产品（可选）                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | 制造者的抵押金额                                                                         |
| \> totalCollateral            | string       | 总抵押金额（接收方+制造者）                                                             |
| \> collateralAtRisk | string       | 担保时需要（可选）                                                           |
| \> makerBalanceThreshold | string       | 制造者余额阈值                                                          |
| \> deadline                   | number         | 到期时间戳（例如：1672387200）                                                           |
| \> makerWallet                | string       | 制造者的钱包（可选）                                                                     |
| \> signature                  | string       | 签名（可选）                                                                          |

**请求示例**

```
GET rfq/smart-trend/recommended-list?vault=xxxxxx
```

### 智能趋势查询

- 注意事项：
  - 请勿在纯查询请求中传递用户的钱包地址。
  - 仅在订阅时传递用户的钱包地址。

```
GET /rfq/smart-trend/quote
```

**输入参数**

| **字段名称**     | **必需** | **类型** | **描述**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | 合约信息                                                                                                   |
| chainId            | true         | int      | 链ID                                                                                                               |
| expiry             | true         | number     | 到期日期的秒级时间戳，例如：1672387200                                                           |
| lowerBarrier       | true         | number   | 下限价格                                                                                                            |
| upperBarrier       | true         | number   | 上限价格                                                                                                            |
| depositAmount      | true         | number   | RFQ购买金额                                                                                                    |
| inputApyDefinition | true         | string   | 底层代码是枚举，指示输入APY的计算方式：OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | 保证年收益率（RISKY为空，protected时必填）                                                      |
| fundingApy         | false        | number   | AAVE年收益率（RISKY为空，protected时必填）                                                            |
| takerWallet        | false        | string   | 查询者的钱包公钥地址信息                                                                           |

**响应参数**


| **字段名称** | **类型**     | **描述**                                |
| ------------ | ------------ | ---------------------------------------- |
| code         | int          | 0 表示返回结果正常                       |
| message      | string       | 异常情况下返回的错误信息                 |
| value        | object       | 如下所示                                 |

对象

| **字段名称**               | **类型**     | **描述**                                                                                   |
| -------------------------- | ------------ | ------------------------------------------------------------------------------------------ |
| rfqId                      | number       | RFQ ID                                                                                     |
| vault                      | string       | 合约地址                                                                                   |
| chainId                    | int          | 链 ID                                                                                      |
| riskType                   | string       | 风险类型：PROTECTED, RISKY                                                                 |
| direction                  | string       | BULLISH, BEARISH                                                                           |
| forCcy                     | string       | 标的货币                                                                                   |
| domCcy                     | string       | 货币对                                                                                     |
| depositCcy                 | string       | 认购货币                                                                                   |
| lowerBarrier               | number       | 下限价格                                                                                   |
| upperBarrier               | number       | 上限价格                                                                                   |
| depositAmount              | number       | RFQ 购买金额                                                                               |
| expiry                     | number       | 到期时间戳（例如：1672387200）                                                             |
| timestamp                  | number       | 当前定价的触发时间；下一个观察开始时间基于此逻辑计算                                       |
| feeRate                    | object       | 交易和结算费率（可选）                                                                     |
| leverageInfo               | object       | 贷款信息（可选）                                                                           |
| relevantDollarPrices       | list[object] | RCH 价格转换和空投计算所需的代币价格（可选）                                               |
| amounts                    | object       | 计算金额（可选）                                                                           |
| apyInfo                    | object       | 年化信息，适用于非 Surge 产品（可选）                                                      |
| oddsInfo                   | object       | 赔率信息，适用于 Surge 产品（可选）                                                        |
| quote                      | object       |                                                                                            |
| \> quoteId                 | number       |                                                                                            |
| \> anchorPrices            | list[string] | 20000000000, 30000000000                                                                   |
| \> makerCollateral         | string       | 做市商的抵押金额                                                                           |
| \> totalCollateral         | string       | 总抵押金额（Taker+Maker）                                                                  |
| \> collateralAtRisk        | string       | 保证时需要（可选）                                                                         |
| \> makerBalanceThreshold   | string       | 做市商余额阈值                                                                             |
| \> deadline                | number       | 到期时间戳（例如：1672387200）                                                             |
| \> makerWallet             | string       | 做市商的钱包（可选）                                                                       |
| \> signature               | string       | 签名（可选）                                                                               |

### 智能趋势获胜概率

在到期前保持在边界内的概率

```
GET rfq/smart-trend/winning-probabilities
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | 标的货币      |
| expiry          | true         | number     | 到期日的秒级时间戳，例如：1672387200 |
| lowerBarrier       | true         | number   | 下限价格 |
| upperBarrier       | true         | number   | 上限价格 |

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回结果正常   |
| message        | string       | 异常情况下返回的错误信息 |
| value          | list[object] | 如下所示                                 |

对象

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | number | 现货价格 |
| timestamp | number |  |
| probabilities | object | 获胜概率  |


**请求示例**

```
GET rfq/smart-trend/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerStrike=xxx&upperStrike=xxx
```

**响应**

```json
{
    "code":0,
    "message":"",
    "value": {
        "spotPrice": 62121,
        "timestamp": 1727080594,
        "probabilities": {
            "probDntStayInRange": null,
            "probBullTrendItmLowerStrike": 0.4,
            "probBullTrendItmUpperStrike": 0.4,
            "probBearTrendItmLowerStrike": 0.6,
            "probBearTrendItmUpperStrike": 0.6,
        }
    }
}
```

## 通用接口

### 删除 RFQ

删除已上链的 RFQ

```
POST rfq/remove
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| rfqId          | true         | number       | .       |

**请求示例**

```
{  
"rfqId":123456  
}
```

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0表示返回结果正常   |
| message        | string       | 异常情况下返回的错误信息 |
| value          | object       | .                               |


### 交易通知

```
POST rfq/trade
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| quotes | true | list[object] |   |
| \> rfqId | true | number |     |
| \> quoteId | true | number |     |
| \> txId | true | string | 交易哈希 |
| code | false | string | 邀请码 |
| walletType | false | string | 钱包类型，如MetaMask、OKX Wallet、Coinbase等 |

**请求示例**

```
{
    "quotes": [
        {
            "rfqId": 123456,
            "quoteId": 333,
```

```json
            "txId": "adsswe"
        }
    ],
    "code": "adsswe"
}
```

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------- |
| code           | int          | 0 表示返回结果正常                       |
| message        | string       | 异常情况下返回的错误信息                 |
| value          | object       | .                                        |

### 行权价列表

```
GET rfq/strike-list
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| indexPrice | true | number |  |
| forCcy | false | string | 对于 BTC-USDT 交易对，在此输入 BTC；如果未提供，将使用默认配置。 |
| domCcy | false | string | 对于 BTC-USDT 交易对，在此输入 USDT；如果未提供，默认为 USDT。 |

**请求示例**

```
{  
"indexPrice": 3750.8，  
"forCcy": "WBTC",  
}
```

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| strikes | list[number] | 默认推荐行权价列表 |

### 到期列表

```
GET rfq/expiry-list
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| vault | true | string | 合约地址 |
| chainId | true | int | 链ID |

**请求示例**

```
{  
"vault": "XXXXXXXXXXXXXX",  
"chainId": 1,  
}
```

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| timestamp | number | 基线开始时间的秒级时间戳 |
| expiries | list[number] | 支持的到期列表，以秒级时间戳表示，例如：1672387200 |

### Aave 年化收益率

```
GET rfq/aave-apy
```

**输入参数**

| **字段名称** | **必填** | **类型** | **描述** |
| --- | --- | --- | --- |
| chainId | 是 | number |     |
| ccy | 是 | string | USDT |

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | 链 ID |
| ccy | string | 存款货币 |
| avgApy | string | 过去30天链上记录的平均 APY |
| currentApy | string | 最新 AAVE APY |
| apyUsed | string | SofaServer 实际用于估算未来利息收入的 APY |
| apyDefinition | string | 对应于 APY 的计算定义 |

**响应示例**

```
{  
"chainId": 1,  
"ccy":"USDT",  
"avgApy":"0.23442",  
"currentApy":"0.23442",  
"apyUsed":"0.23442",  
"apyDefinition":"AAVE_LENDING_APY"  
}
```

### Apy

```
GET rfq/apy
```

**输入参数**


| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |
| apyDefinition | false | string | 与APY对应的计算定义；默认为AaveLendingApy |

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | 链ID |
| ccy | string | 存款货币 |
| avgApy | string | 过去30天链上记录的平均APY |
| currentApy | string | 最新APY |
| apyUsed | string | SofaServer中实际用于估算未来利息收入的APY |
| apyDefinition | string | 与APY对应的计算定义 |

**响应示例**

```
{  
"chainId": 1,  
"ccy":"USDT",  
"avgApy":"0.23442",  
"currentApy":"0.23442",  
"apyUsed":"0.23442",  
"apyDefinition":"AAVE_LENDING_APY"  
}
```

### 获取钱包头寸

```
POST /rfq/position-list
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 合约地址集合；如果未提供，将查询所有合约。 |
| claimed | false | boolean | 是否已赎回；如果未提供，将查询所有状态的头寸。 |
| expired | false | boolean | 是否已过期；如果未提供，将查询所有状态的头寸。 |
| concealed | false | boolean | 是否隐藏；如果未提供，将查询所有状态的头寸。 |
| positiveReturn | false | boolean | 赎回金额是否大于0；如果未提供，将查询所有头寸。 |
| positiveProfit | false | boolean | 收益是否超过本金；如果未提供，将查询所有头寸。 |
| limit | false | Int | 查询数量；默认为100，最大为300。 |
| startDateTime | false | number | 对应的时间戳（秒，包含），例如：1672387200。 |
| endDateTime | false | number | 对应的时间戳（秒，包含），例如：1672387200。 |
| orderBy | false | string | "createdAt" 或 "return"，排序方式："createdAt"（更新时间，默认）或 "return"（收益）。 |
| orderDirection | false | string | "desc" 或 "asc"，默认为 "desc"（降序）。 |
| wallet | false | string | 钱包地址（如果为空，将查询所有钱包地址）。 |

注意：如果在指定的 `startDateTime` 和 `endDateTime` 内结果数量超过300，可以使用轮询（此时 `orderBy` 必须设置为 `"createdAt"`）继续查询。第n次轮询请求的 `endDateTime` 参数应设置为第(n-1)次轮询结果中最后一条记录的 `createdAt`。获取所有数据后，应根据 `id` 字段去重。

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 如下所示 |

对象

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string | 与头寸对应的产品ID（与The Graph数据中的 `productId` 匹配）。此ID在链+保险库维度内是唯一的，可用于查询头寸余额。 |
| positionId | string | 头寸ID，在链维度内是唯一的。 |
| product | object | 产品信息。 |
| wallet | string | 钱包地址。 |
| createdAt | number | 对应的时间戳（秒），例如：1672387200。 |
| updatedAt | number | 对应的时间戳（秒），例如：1672387200。 |
| claimed | boolean | 头寸是否已赎回。 |
| takerAllocationRate | number | 基于计算时间，估算所有者可从投注池中获取的比例。在到期前，这是一个估算值。 |
| triggerTime | number | 对于Rangebound，为首次突破时间；对于非Rangebound，为结算时间，单位为秒。 |
| triggerPrice | number | 对于Rangebound，为首次突破价格；对于非Rangebound，为结算价格。 |
| feeRate | object | 费率。 |
| leverageInfo | object | 贷款信息（可选）。 |
| relevantDollarPrices | list[object] | RCH价格转换和空投计算所需的代币价格。 |
| amounts | object | 计算的金额。 |
| apyInfo | object | 年化信息，适用于非Surge产品。 |
| oddsInfo | object | 概率信息，适用于Surge产品。 |
| claimParams | object | 索赔参数信息。 |

**响应示例**

```
{
    "value": [{
        ...
    }]
}
```

### 获取钱包交易

```
POST /rfq/transaction-list
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 合约地址集合；如果未提供，将查询所有合约。 |
| limit | false | Int | 查询数量；默认为100，最大为300。 |
| startDateTime | false | number | 对应的时间戳（秒）（包含），例如：1672387200。 |
| endDateTime | false | number | 对应的时间戳（秒）（包含），例如：1672387200。 |
| orderDirection | false | string | "desc"或"asc"，默认为"desc"（降序）。 |
| taker | false | string | 接收方钱包地址 |
| maker | false | string | 发送方钱包地址 |
| claimParams | false | object | 赎回参数，对应position-list数据中的claimParams字段。 |
| hash | false | string | 交易哈希 |

注意：taker、maker和claimParams参数可用于联合查询，以找到与某个仓位对应的交易记录（一个仓位可能对应多个交易）（并非所有参数都是必需的）。
如果在指定的`startDateTime`和`endDateTime`内结果数量超过300，可以使用轮询（此时必须将`orderBy`设置为`"createdAt"`）继续查询。第n次轮询请求的`endDateTime`参数应设置为第(n-1)次轮询结果中最后一条记录的`createdAt`。获取所有数据后，应根据`id`字段去重。

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 如下所示 |

对象

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | 交易哈希 |
| takerWallet | string | 接受者钱包地址。 |
| makerWallet | string | 创建者钱包地址。 |
| product | object | 产品信息。 |
| createdAt | number | 对应的时间戳，以秒为单位，例如：1672387200。 |
| takerAllocationRate | number | 基于计算时间，估算所有者可以从投注池中获取的比例。在到期前，这是一个估算值。 |
| triggerTime | number | 对于Rangebound，为首次突破时间；对于非Rangebound，为结算时间，以秒为单位。 |
| triggerPrice | number | 对于Rangebound，为首次突破价格；对于非Rangebound，为结算价格。 |
| feeRate | object | 费率。 |
| leverageInfo | object | 贷款信息（可选）。 |
| relevantDollarPrices | list[object] | RCH价格转换和空投计算所需的代币价格。 |
| amounts | object | 计算出的金额。 |
| apyInfo | object | 年化信息，适用于非Surge产品。 |
| oddsInfo | object | 赔率信息，适用于Surge产品。 |

**响应示例**

```
{
    "value": [{
        ...
    }]
}
```

### 隐藏亏损头寸

```
POST rfq/position/conceal
```

**输入参数**

| **字段名称** | **必填** | **类型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | int | 链 ID |
| positionIds | true | List[string] | positionId 列表，最多 20 个 |

**请求示例**

```
{
    "positionIds": [
        "aaaa","bbbb"
    ],
    "chainId": 1
}
```

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回结果正常   |
| message        | string       | 异常情况下返回的错误信息 |
| value          | object       | .                               |


### 获取待处理交易

这主要是为了解决底层仓位数据同步问题。此处获取的数据可能会消失（由于区块链分叉竞争）。

```
POST rfq/transactions/pending
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 合约地址集合；如果未提供，将查询所有合约。 |
| taker | false | string | 承接方钱包地址 |
| maker | false | string | 发起方钱包地址 |

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 如下所示 |

对象

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | 交易哈希 |
| takerWallet | string | 承接方钱包地址。 |
| makerWallet | string | 发起方钱包地址。 |
| product | object | 产品信息。 |
| createdAt | number | 对应的时间戳（秒），例如：1672387200。 |
| takerAllocationRate | number | 基于计算时间，估算所有者可以从投注池中获得的比例。在到期前，这是一个估算值。 |
| triggerTime | number | 对于Rangebound，首次突破时间；对于非Rangebound，结算时间，以秒为单位。 |
| triggerPrice | number | 对于Rangebound，首次突破价格；对于非Rangebound，结算价格。 |
| feeRate | object | 费率。 |
| leverageInfo | object | 贷款信息（可选）。 |
| relevantDollarPrices | list[object] | RCH价格转换和空投计算所需的代币价格。 |
| amounts | object | 计算的金额。 |
| apyInfo | object | 年化信息，适用于非Surge产品。 |
| oddsInfo | object | 赔率信息，适用于Surge产品。 |

### 获取RCH空投历史记录

```
GET rfq/airdrop/history
```

**输入参数**

| **字段名称** | **必需** | **类型** | **描述** |
| --- | --- | --- | --- |
| wallet | true | string | 钱包地址 |
| startDateTime | true | number | 对应的时间戳（秒）（包含），例如：1672387200。 |
| endDateTime | true | number | 对应的时间戳（秒）（包含），例如：1672387200。 |
| orderBy | false | string | "dateTime" 或 "rch"，排序方式："dateTime"（空投时间，默认）或 "rch"（数量）。 |
| orderDirection | false | string | "desc" 或 "asc"，默认为 "desc"（降序）。 |

**响应参数**

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 如下所示 |

对象

| **字段名称** | **类型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| dateTime | long | 当前时间戳（秒），例如：1672387200。 |
| wallet | string | 钱包地址 |
| volume | string | 交易量 |
| rch | string | 空投数量，原始值，需要除以 1e18 |
| merkleProof | string | Merkle 证明 |
