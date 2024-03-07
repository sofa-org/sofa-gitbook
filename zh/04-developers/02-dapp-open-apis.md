# Open APIs for DAPP开发者

我们为 Sofa.org 提供了合约产品定价的功能，通过向 MarketMaker 询价，然后整合最优的定价结果返回给 Sofa.org，然后用户可以选择是否交易，由于需要根据合约对接不同的 MarketMaker，所以可上线的产品需要按合约类型区分。

## 推荐 DNT RFQ 询价

- 说明
  - 仅支持同一个 vault+chainId 组成的 rfq 列表

```
POST /rfq/dnt/recommended-quote
```

**入参**

| 字段名               | 必填          | 类型   | 描述                                                                                                                    |
| -------------------- | ------------- | ------ | ----------------------------------------------------------------------------------------------------------------------- |
| list                 | array[object] |        |                                                                                                                         |
| > vault              | true          | string | 合约信息                                                                                                                |
| > chainId            | true          | int    | 链 ID                                                                                                                   |
| > expiry             | true          | long   | 到期日对应的秒级时间戳，例如 1672387200                                                                                 |
| > lowerBarrier       | true          | number | 下方价格                                                                                                                |
| > upperBarrier       | true          | number | 上方价格                                                                                                                |
| > depositAmount      | true          | number | rfq 申购金额                                                                                                            |
| > inputApyDefinition | true          | string | 底层代码是 Enum 表明入参的 APY 具体是怎么计算的_OptimusDefaultAPY_, _BinanceDntAPY_, _AaveLendingAPR_, _AaveLendingAPY_ |
| > protectedApy       | false         | number | 保底年化（RISKY 时为空，protected 时必填）                                                                              |
| > fundingApy         | false         | number | AAVE 年化（RISKY 时为空，protected 时必填）                                                                             |

入参示例

```json
{
    "list":[
        {
            "vault":"0x0E3a24ed24fBfF5406C8529fBe3Cc4DFE12b1cDB",
            "chainId":80001,
            "lowerBarrier":2775,
            "upperBarrier":2825,
            "expiry":1708416000,
            "depositAmount":1000,
            "protectedApy":0.01,
            "fundingApy":"0.1098",
            "inputApyDefinition":"AaveLendingAPY"
        }
    ]
}
```

**出参**

| 字段名  | 类型         | 描述                 |
| ------- | ------------ | -------------------- |
| code    | int          | 0 表示返回结果正常   |
| message | string       | 异常时返回的错误信息 |
| value   | list[object] |                      |

Object

| 字段名                       | 类型         | 描述                                                     |
| ---------------------------- | ------------ | -------------------------------------------------------- |
| vault                        | string       | 合约地址                                                 |
| chainId                      | int          | 链 ID                                                    |
| riskType                     | string       | 风险类型：PROTECTED,RISKY                                |
| forCcy                       | string       | 标的物币种                                               |
| domCcy                       | string       | 币对                                                     |
| depositCcy                   | string       | 申购币种                                                 |
| lowerBarrier                 | number       | 下方价格                                                 |
| upperBarrier                 | number       | 上方价格                                                 |
| depositAmount                | number       | rfq 申购金额                                             |
| expiry                       | long         | 到期日对应的秒级时间戳，例如 1672387200                  |
| protectedReturnAmount        | number       | 保底收益的总金额，单位与 depositCcy 一致                 |
| fundingAmount                | number       | 预估的 AAVE 在该期间的收益率；非保本型为零               |
| enhancedReturnAmount         | number       | 未敲出时额外收益的总金额，单位与 depositCcy 一致         |
| timestamp                    | long         | 目前定价对应的触发时间；下一个观察开始时间基于此逻辑计算 |
| observationStart             | long         | 目前产品根据 timestamp 预估的开始观察敲入敲出时间        |
| quote                        | object       |                                                          |
| > anchorPrices               | list[string] | 20000000000, 30000000000                                 |
| > makerCollateral            | string       | Maker 对赌抵押的金额                                     |
| > totalCollateral            | string       | 总的质押的金额（Taker+Maker）                            |
| > collateralAtRiskPercentage | string       | 保底时必填（可空）                                       |
| > deadline                   | long         | 过期时间秒级时间戳 例如 1672387200                       |
| > makerWallet                | string       | maker 的 wallet（可空）                                  |
| > signature                  | string       | 签名（可空）                                             |

出参示例

```json
{
    "succ":true,
    "code":0,
    "message":"",
    "value":[
        {
            "chainId":80001,
            "vault":"0x0E3a24ed24fBfF5406C8529fBe3Cc4DFE12b1cDB",
            "riskType":"PROTECTED",
            "forCcy":"WETH",
            "domCcy":"USDT",
            "depositCcy":"USDT",
            "lowerBarrier":"2775",
            "upperBarrier":"2825",
            "depositAmount":"1000",
            "expiry":"1708416000",
            "timestamp":"1708142778",
            "protectedReturnAmount":"0.08219178",
            "fundingAmount":"0.90246575",
            "enhancedReturnAmount":"3.69018697",
            "observationStart":"1708156800",
            "quote":{
                "anchorPrices":[
                    "2775000000",
                    "2825000000"
                ],
                "makerCollateral":"2869913",
                "totalCollateral":"1002869913",
                "collateralAtRiskPercentage":"3631",
                "deadline":"1708143378",
                "makerWallet":"0xDFD6f6a6c1AB0184a483F1c7f691f1ea6B810e55",
                "signature":null
            }
        },
        {
            "chainId":80001,
            "vault":"0x0E3a24ed24fBfF5406C8529fBe3Cc4DFE12b1cDB",
            "riskType":"PROTECTED",
            "forCcy":"WETH",
            "domCcy":"USDT",
            "depositCcy":"USDT",
            "lowerBarrier":"2775",
            "upperBarrier":"2825",
            "depositAmount":"1000",
            "expiry":"1708761600",
            "timestamp":"1708142781",
            "protectedReturnAmount":"0.19178082",
            "fundingAmount":"2.10575342",
            "enhancedReturnAmount":"21.6891866",
            "observationStart":"1708156800",
            "quote":{
                "anchorPrices":[
                    "2775000000",
                    "2825000000"
                ],
                "makerCollateral":"19775214",
                "totalCollateral":"1019775214",
                "collateralAtRiskPercentage":"21123",
                "deadline":"1708143381",
                "makerWallet":"0xDFD6f6a6c1AB0184a483F1c7f691f1ea6B810e55",
                "signature":null
            }
        }
    ]
}
```

## 询价

- 说明
  - 仅询价时，不传用户钱包地址（wallet）
  - 申购时，传用户钱包地址（wallet）

```
GET /rfq/dnt/quote
```

**入参**

| 字段名             | 必填  | 类型   | 描述                                                                                                                    |
| ------------------ | ----- | ------ | ----------------------------------------------------------------------------------------------------------------------- |
| vault              | true  | string | 合约信息                                                                                                                |
| chainId            | true  | int    | 链 ID                                                                                                                   |
| expiry             | true  | long   | 到期日对应的秒级时间戳，例如 1672387200                                                                                 |
| lowerBarrier       | true  | number | 下方价格                                                                                                                |
| upperBarrier       | true  | number | 上方价格                                                                                                                |
| depositAmount      | true  | number | rfq 申购金额                                                                                                            |
| inputApyDefinition | true  | string | 底层代码是 Enum 表明入参的 APY 具体是怎么计算的_OptimusDefaultAPY_, _BinanceDntAPY_, _AaveLendingAPR_, _AaveLendingAPY_ |
| protectedApy       | false | number | 保底年化（RISKY 时为空，protected 时必填）                                                                              |
| fundingApy         | false | number | AAVE 年化（RISKY 时为空，protected 时必填）                                                                             |
| takerWallet        | false | string | 询价方钱包公共地址信息                                                                                                  |

**出参**

| 字段名  | 类型         | 描述                 |
| ------- | ------------ | -------------------- |
| code    | int          | 0 表示返回结果正常   |
| message | string       | 异常时返回的错误信息 |
| value   | list[object] |                      |

Object

| 字段名                       | 类型         | 描述                                                     |
| ---------------------------- | ------------ | -------------------------------------------------------- |
| vault                        | string       | 合约地址                                                 |
| chainId                      | int          | 链 ID                                                    |
| riskType                     | string       | 风险类型：PROTECTED,RISKY                                |
| forCcy                       | string       | 标的物币种                                               |
| domCcy                       | string       | 币对                                                     |
| depositCcy                   | string       | 申购币种                                                 |
| lowerBarrier                 | number       | 下方价格                                                 |
| upperBarrier                 | number       | 上方价格                                                 |
| depositAmount                | number       | rfq 申购金额                                             |
| expiry                       | long         | 到期日对应的秒级时间戳，例如 1672387200                  |
| protectedReturnAmount        | number       | 保底收益的总金额，单位与 depositCcy 一致                 |
| fundingAmount                | number       | 预估的 AAVE 在该期间的收益率；非保本型为零               |
| enhancedReturnAmount         | number       | 未敲出时额外收益的总金额，单位与 depositCcy 一致         |
| timestamp                    | long         | 目前定价对应的触发时间；下一个观察开始时间基于此逻辑计算 |
| observationStart             | long         | 目前产品根据 timestamp 预估的开始观察敲入敲出时间        |
| quote                        | object       |                                                          |
| > anchorPrices               | list[string] | 20000000000, 30000000000                                 |
| > makerCollateral            | string       | Maker 对赌抵押的金额                                     |
| > totalCollateral            | string       | 总的质押的金额（Taker+Maker）                            |
| > collateralAtRiskPercentage | string       | 保底时必填（可空）                                       |
| > deadline                   | long         | 过期时间秒级时间戳 例如 1672387200                       |
| > makerWallet                | string       | maker 的 wallet（可空）                                  |
| > signature                  | string       | 签名（可空）                                             |

