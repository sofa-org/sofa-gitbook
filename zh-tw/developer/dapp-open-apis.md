
# DApp 開放 API

我們現有的 dApp 在 sofa.org 上是一個關鍵的工作流程功能，也是用戶與 SOFA 協議互動的主要方式。然而，我們鼓勵其他開發者通過他們自己的 dApp 來啟用訪問並連接到 SOFA，以最大化我們生態系統的增長。我們將這些視為我們的「經紀人」合作夥伴，並鼓勵有興趣的各方聯繫 SOFA 團隊以獲取進一步的 API 信息。

## DNT

### 推薦的 DNT RFQ 列表查詢

```
GET /rfq/dnt/recommended-list
```

**輸入參數**

| **欄位名稱**       | **必需**  | **類型** | **描述**                                                                                           |
| -------------------- | ------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| vault              | true          | string   | 合約信息                                                                                                   |
| chainId            | true          | int      | 鏈 ID                                                                                                               |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | list[object] | 如下所示                                               |

物件

| **欄位名稱**               | **類型**     | **描述**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | 鏈 ID                                                                                          |
| vault                        | string       | 合約地址                                                                                  |
| riskType                     | string       | 風險類型：PROTECTED, RISKY                                                                       |
| forCcy                       | string       | 標的貨幣                                                                               |
| domCcy                       | string       | 貨幣對                                                                                     |
| depositCcy                   | string       | 認購貨幣                                                                             |
| lowerBarrier                 | number       | 下限價格                                                                                       |
| upperBarrier                 | number       | 上限價格                                                                                       |
| depositAmount                | number       | RFQ 購買金額                                                                               |
| expiry                       | number         | 到期時間戳（例如，1672387200）                                                               |
| timestamp                    | number         | 當前定價的觸發時間；下一次觀察開始時間基於此邏輯計算 |
| observationStart             | number         | 根據時間戳估算的敲入/敲出觀察開始時間                        |
| feeRate             | object         | 交易和結算費率（可選）                        |
| leverageInfo             | object         | 貸款信息（可選）                        |
| relevantDollarPrices             | list[object]         | RCH 價格轉換和空投計算所需的代幣價格（可選）                       |
| amounts             | object         | 計算的金額（可選）                       |
| apyInfo             | object         | 年化信息，適用於非 Surge 產品（可選）                     |
| oddsInfo             | object         | 賠率信息，適用於 Surge 產品（可選）                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | 做市商的抵押金額                                                                         |
| \> totalCollateral            | string       | 總抵押金額（Taker+Maker）                                                             |
| \> collateralAtRisk | string       | 保證時需要（可選）                                                           |
| \> makerBalanceThreshold | string       | 做市商餘額門檻                                                          |
| \> deadline                   | number         | 到期時間戳（例如，1672387200）                                                           |
| \> makerWallet                | string       | 做市商錢包（可選）                                                                     |
| \> signature                  | string       | 簽名（可選）                                                                          |

**請求範例**

```
GET rfq/dnt/recommended-list?vault=xxxxxx&chainId=1
```

**回應**

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

### DNT 查詢

- 注意事項：
  - 請不要在純查詢請求中傳遞用戶的錢包地址。
  - 只有在訂閱時才應傳遞用戶的錢包地址。

```
GET /rfq/dnt/quote
```

**輸入參數**

| **欄位名稱**     | **必需** | **類型** | **描述**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | 合約信息                                                                                                   |
| chainId            | true         | int      | 鏈 ID                                                                                                               |
| expiry             | true         | number     | 到期日期的秒級時間戳，例如，1672387200                                                           |
| lowerBarrier       | true         | number   | 下限價格                                                                                                            |
| upperBarrier       | true         | number   | 上限價格                                                                                                            |
| depositAmount      | true         | number   | RFQ 購買金額                                                                                                    |
| inputApyDefinition | true         | string   | 底層代碼是 Enum，指示如何計算輸入的 APY：OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | 保證年收益率（RISKY 為空，protected 必填）                                                      |
| fundingApy         | false        | number   | AAVE 年收益率（RISKY 為空，protected 必填）                                                            |
| takerWallet        | false        | string   | 查詢者的錢包公鑰信息                                                                           |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | object       | 如下所示                                 |

物件

| **欄位名稱**               | **類型**     | **描述**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | 合約地址                                                                                  |
| chainId                      | int          | 鏈 ID                                                                                          |
| riskType                     | string       | 風險類型：PROTECTED, RISKY                                                                       |
| forCcy                       | string       | 標的貨幣                                                                               |
| domCcy                       | string       | 貨幣對                                                                                     |
| depositCcy                   | string       | 認購貨幣                                                                             |
| lowerBarrier                 | number       | 下限價格                                                                                       |
| upperBarrier                 | number       | 上限價格                                                                                       |
| depositAmount                | number       | RFQ 購買金額                                                                               |
| expiry                       | number         | 到期時間戳（例如，1672387200）                                                               |
| timestamp                    | number         | 當前定價的觸發時間；下一次觀察開始時間基於此邏輯計算 |
| observationStart             | number         | 根據時間戳估算的敲入/敲出觀察開始時間                        |
| feeRate             | object         | 交易和結算費率（可選）                        |
| leverageInfo             | object         | 貸款信息（可選）                        |
| relevantDollarPrices             | list[object]         | RCH 價格轉換和空投計算所需的代幣價格（可選）                       |
| amounts             | object         | 計算的金額（可選）                       |
| apyInfo             | object         | 年化信息，適用於非 Surge 產品（可選）                     |
| oddsInfo             | object         | 賠率信息，適用於 Surge 產品（可選）                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | 做市商的抵押金額                                                                         |
| \> totalCollateral            | string       | 總抵押金額（Taker+Maker）                                                             |
| \> collateralAtRisk | string       | 保證時需要（可選）                                                           |
| \> makerBalanceThreshold | string       | 做市商餘額門檻                                                          |
| \> deadline                   | number         | 到期時間戳（例如，1672387200）                                                           |
| \> makerWallet                | string       | 做市商錢包（可選）                                                                     |
| \> signature                  | string       | 簽名（可選）                                                                          |

### DNT 贏的概率

在到期前保持在界限內的概率

```
GET rfq/dnt/winning-probabilities
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | 標的貨幣      |
| expiry          | true         | number     | 到期日期的秒級時間戳，例如，1672387200 |
| lowerBarrier       | true         | number   | 下限價格 |
| upperBarrier       | true         | number   | 上限價格 |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | list[object] | 如下所示                                 |

物件

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | number | 現貨價格 |
| timestamp | number |  |
| probabilities | object | 贏的概率  |

**請求範例**

```
GET rfq/dnt/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerBarrier=xxx&upperBarrier=xxx
```

**回應**

```json
{
    "code":0,
    "message":"",
    "value": {
        "spotPrice": 62121,
        "timestamp": 1727080594,
        "probabilities": {
            "probDntStayInRange": 0.4
            "probBullTrendItmLowerStrike": null,
            "probBullTrendItmUpperStrike": null,
            "probBearTrendItmLowerStrike": null,
            "probBearTrendItmUpperStrike": null,
        }
    }
}
```

## 智能趨勢

### 推薦的智能趨勢 RFQ 列表查詢

```
GET rfq/smart-trend/recommended-list
```

**輸入參數**

| **欄位名稱**       | **必需**  | **類型** | **描述**                                         |
| -------------------- | ------------- | -------- | ------------------------------------------------------------ |
| vault              | true          | string   | 合約信息                               |
| chainId            | true          | int      | 鏈 ID                    |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | list[object] | 如下所示                                 |

物件

| **欄位名稱**               | **類型**     | **描述**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | 鏈 ID                                                                                          |
| vault                        | string       | 合約地址                                                                                  |
| riskType                     | string       | 風險類型：PROTECTED, RISKY                                                                       |
| direction | string | BULLISH,BEARISH |   
| forCcy                       | string       | 標的貨幣                                                                               |
| domCcy                       | string       | 貨幣對                                                                                     |
| depositCcy                   | string       | 認購貨幣                                                                             |
| lowerBarrier                 | number       | 下限價格                                                                                       |
| upperBarrier                 | number       | 上限價格                                                                                       |
| depositAmount                | number       | RFQ 購買金額                                                                               |
| expiry                       | number         | 到期時間戳（例如，1672387200）                                                               |
| timestamp                    | number         | 當前定價的觸發時間；下一次觀察開始時間基於此邏輯計算 |
| feeRate             | object         | 交易和結算費率（可選）                        |
| leverageInfo             | object         | 貸款信息（可選）                        |
| relevantDollarPrices             | list[object]        | RCH 價格轉換和空投計算所需的代幣價格（可選）                       |
| amounts             | object         | 計算的金額（可選）                       |
| apyInfo             | object         | 年化信息，適用於非 Surge 產品（可選）                     |
| oddsInfo             | object         | 賠率信息，適用於 Surge 產品（可選）                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | 做市商的抵押金額                                                                         |
| \> totalCollateral            | string       | 總抵押金額（Taker+Maker）                                                             |
| \> collateralAtRisk | string       | 保證時需要（可選）                                                           |
| \> makerBalanceThreshold | string       | 做市商餘額門檻                                                          |
| \> deadline                   | number         | 到期時間戳（例如，1672387200）                                                           |
| \> makerWallet                | string       | 做市商錢包（可選）                                                                     |
| \> signature                  | string       | 簽名（可選）                                                                          |

**請求範例**

```
GET rfq/smart-trend/recommended-list?vault=xxxxxx
```

### 智能趨勢查詢

- 注意事項：
  - 請不要在純查詢請求中傳遞用戶的錢包地址。
  - 只有在訂閱時才應傳遞用戶的錢包地址。

```
GET /rfq/smart-trend/quote
```

**輸入參數**

| **欄位名稱**     | **必需** | **類型** | **描述**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | 合約信息                                                                                                   |
| chainId            | true         | int      | 鏈 ID                                                                                                               |
| expiry             | true         | number     | 到期日期的秒級時間戳，例如，1672387200                                                           |
| lowerBarrier       | true         | number   | 下限價格                                                                                                            |
| upperBarrier       | true         | number   | 上限價格                                                                                                            |
| depositAmount      | true         | number   | RFQ 購買金額                                                                                                    |
| inputApyDefinition | true         | string   | 底層代碼是 Enum，指示如何計算輸入的 APY：OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | 保證年收益率（RISKY 為空，protected 必填）                                                      |
| fundingApy         | false        | number   | AAVE 年收益率（RISKY 為空，protected 必填）                                                            |
| takerWallet        | false        | string   | 查詢者的錢包公鑰信息                                                                           |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | object       | 如下所示                                 |

物件

| **欄位名稱**               | **類型**     | **描述**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | 合約地址                                                                                  |
| chainId                      | int          | 鏈 ID                                                                                          |
| riskType                     | string       | 風險類型：PROTECTED, RISKY                                                                       |
| direction                    | string       | BULLISH,BEARISH | 
| forCcy                       | string       | 標的貨幣                                                                               |
| domCcy                       | string       | 貨幣對                                                                                     |
| depositCcy                   | string       | 認購貨幣                                                                             |
| lowerBarrier                 | number       | 下限價格                                                                                       |
| upperBarrier                 | number       | 上限價格                                                                                       |
| depositAmount                | number       | RFQ 購買金額                                                                               |
| expiry                       | number         | 到期時間戳（例如，1672387200）                                                               |
| timestamp                    | number         | 當前定價的觸發時間；下一次觀察開始時間基於此邏輯計算 |
| feeRate             | object         | 交易和結算費率（可選）                        |
| leverageInfo             | object         | 貸款信息（可選）                        |
| relevantDollarPrices             | list[object]         | RCH 價格轉換和空投計算所需的代幣價格（可選）                       |
| amounts             | object         | 計算的金額（可選）                       |
| apyInfo             | object         | 年化信息，適用於非 Surge 產品（可選）                     |
| oddsInfo             | object         | 賠率信息，適用於 Surge 產品（可選）                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | 做市商的抵押金額                                                                         |
| \> totalCollateral            | string       | 總抵押金額（Taker+Maker）                                                             |
| \> collateralAtRisk | string       | 保證時需要（可選）                                                           |
| \> makerBalanceThreshold | string       | 做市商餘額門檻                                                          |
| \> deadline                   | number         | 到期時間戳（例如，1672387200）                                                           |
| \> makerWallet                | string       | 做市商錢包（可選）                                                                     |
| \> signature                  | string       | 簽名（可選）                                                                          |

### 智能趨勢贏的概率

在到期前保持在界限內的概率

```
GET rfq/smart-trend/winning-probabilities
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | 標的貨幣      |
| expiry          | true         | number     | 到期日期的秒級時間戳，例如，1672387200 |
| lowerBarrier       | true         | number   | 下限價格 |
| upperBarrier       | true         | number   | 上限價格 |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | list[object] | 如下所示                                 |

物件

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | number | 現貨價格 |
| timestamp | number |  |
| probabilities | object | 贏的概率  |


**請求範例**

```
GET rfq/smart-trend/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerStrike=xxx&upperStrike=xxx
```

**回應**

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

## 通用介面

### 刪除 RFQ

刪除已上鏈的 RFQ

```
POST rfq/remove
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| rfqId          | true         | number       | .       |

**請求範例**

```
{  
"rfqId":123456  
}
```

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | object       | .                               |


### 交易通知

```
POST rfq/trade
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| quotes | true | list[object] |   |
| \> rfqId | true | number |     |
| \> quoteId | true | number |     |
| \> txId | true | string | 交易哈希 |
| code | false | string | 邀請碼 |
| walletType | false | string | 錢包類型，如 MetaMask、OKX Wallet、Coinbase 等。 |

**請求範例**

```
{
    "quotes": [
        {
            "rfqId": 123456,
            "quoteId": 333,
            "txId": "adsswe"
        }
    ],
    "code": "adsswe"
}
```

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | object       | .                               |

### 行權價列表

```
GET rfq/strike-list
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| indexPrice | true | number |  |
| forCcy | false | string | 對於 BTC-USDT 交易對，這裡輸入 BTC；如果未提供，將使用默認配置。 |
| domCcy | false | string | 對於 BTC-USDT 交易對，這裡輸入 USDT；如果未提供，默認為 USDT。 |


**請求範例**

```
{  
"indexPrice": 3750.8，  
"forCcy": "WBTC",  
}
```

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| strikes | list[number] | 默認推薦的行權價列表 |

### 到期列表

```
GET rfq/expiry-list
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| vault | true | string | 合約地址 |
| chainId | true | int | 鏈 ID |


**請求範例**

```
{  
"vault": "XXXXXXXXXXXXXX",  
"chainId": 1,  
}
```

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| timestamp | number | 基準開始時間的秒級時間戳 |
| expiries | list[number] | 支持的到期列表的秒級時間戳，例如，1672387200 |

### Aave apy

```
GET rfq/aave-apy
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | 鏈 ID |
| ccy | string | 存款貨幣 |
| avgApy | string | 過去 30 天在鏈上記錄的平均 APY |
| currentApy | string | 最新的 AAVE APY |
| apyUsed | string | SofaServer 實際用於估算未來利息收入的 APY |
| apyDefinition | string | 對應於 APY 的計算定義 |

**回應範例**

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

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |
| apyDefinition | false | string | 對應於 APY 的計算定義；默認為 AaveLendingApy |


**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | 鏈 ID |
| ccy | string | 存款貨幣 |
| avgApy | string | 過去 30 天在鏈上記錄的平均 APY |
| currentApy | string | 最新的 APY |
| apyUsed | string | SofaServer 實際用於估算未來利息收入的 APY |
| apyDefinition | string | 對應於 APY 的計算定義 |

**回應範例**

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

### 獲取錢包持倉

```
POST /rfq/position-list
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 合約地址集合；如果未提供，將查詢所有合約。 |
| claimed | false | boolean | 是否已贖回；如果未提供，將查詢所有狀態的持倉。 |
| expired | false | boolean | 是否已過期；如果未提供，將查詢所有狀態的持倉。 |
| concealed | false | boolean | 是否隱藏；如果未提供，將查詢所有狀態的持倉。 |
| positiveReturn | false | boolean | 贖回金額是否大於 0；如果未提供，將查詢所有持倉。 |
| positiveProfit | false | boolean | 回報是否超過本金；如果未提供，將查詢所有持倉。 |
| limit | false | Int | 查詢數量；默認為 100，最大為 300。 |
| startDateTime | false | number | 對應的秒級時間戳（包含），例如，1672387200。 |
| endDateTime | false | number | 對應的秒級時間戳（包含），例如，1672387200。 |
| orderBy | false | string | "createdAt" 或 "return"，排序方式："createdAt"（更新時間，默認）或 "return"（回報）。 |
| orderDirection | false | string | "desc" 或 "asc"，默認為 "desc"（降序）。 |
| wallet | false | string | 錢包地址（如果為空，將查詢所有錢包地址）。 |

注意：如果在指定的 `startDateTime` 和 `endDateTime` 內的結果數量超過 300，可以使用輪詢（此時 `orderBy` 必須設置為 `"createdAt"`）繼續查詢。第 n 次輪詢請求的 `endDateTime` 參數應設置為 (n-1) 次輪詢結果中最後一條記錄的 `createdAt`。獲取所有數據後，應根據 `id` 字段去重。

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 如下所示 |

物件

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string | 與持倉對應的產品 ID（與 The Graph 數據中的 `productId` 匹配）。此 ID 在鏈 + vault 維度內唯一，可用於查詢持倉餘額。 |
| positionId | string | 持倉 ID，在鏈維度內唯一。 |
| product | object | 產品信息。 |
| wallet | string | 錢包地址。 |
| createdAt | number | 對應的秒級時間戳，例如，1672387200。 |
| updatedAt | number | 對應的秒級時間戳，例如，1672387200。 |
| claimed | boolean | 持倉是否已贖回。 |
| takerAllocationRate | number | 根據計算時間，估算持有人可從投注池中獲得的比例。到期前，這是估算值。 |
| triggerTime | number | 對於 Rangebound，首次突破時間；對於非 Rangebound，結算時間，單位為秒。 |
| triggerPrice | number | 對於 Rangebound，首次突破價格；對於非 Rangebound，結算價格。 |
| feeRate | object | 費率。 |
| leverageInfo | object | 貸款信息（可選）。 |
| relevantDollarPrices | list[object] | RCH 價格轉換和空投計算所需的代幣價格。 |
| amounts | object | 計算的金額。 |
| apyInfo | object | 年化信息，適用於非 Surge 產品。 |
| oddsInfo | object | 賠率信息，適用於 Surge 產品。 |
| claimParams | object | 贖回參數信息。 |

**回應範例**

```
{
    "value": [{
        ...
    }]
}
```

### 獲取錢包交易

```
POST /rfq/transaction-list
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 合約地址集合；如果未提供，將查詢所有合約。 |
| limit | false | Int | 查詢數量；默認為 100，最大為 300。 |
| startDateTime | false | number | 對應的秒級時間戳（包含），例如，1672387200。 |
| endDateTime | false | number | 對應的秒級時間戳（包含），例如，1672387200。 |
| orderDirection | false | string | "desc" 或 "asc"，默認為 "desc"（降序）。 |
| taker | false | string | Taker 錢包地址 |
| maker | false | string | Maker 錢包地址 |
| claimParams | false | object | 贖回參數，對應於 position-list 數據中的 claimParams 字段。 |
| hash | false | string | 交易哈希 |

注意：Taker、maker 和 claimParams 參數可用於聯合查詢以查找與持倉對應的交易記錄（一個持倉可能對應多筆交易）（並非所有參數都必需）。
如果在指定的 `startDateTime` 和 `endDateTime` 內的結果數量超過 300，可以使用輪詢（此時 `orderBy` 必須設置為 `"createdAt"`）繼續查詢。第 n 次輪詢請求的 `endDateTime` 參數應設置為 (n-1) 次輪詢結果中最後一條記錄的 `createdAt`。獲取所有數據後，應根據 `id` 字段去重。

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 如下所示 |

物件

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | 交易哈希 |
| takerWallet | string | taker 錢包地址。 |
| makerWallet | string | maker 錢包地址。 |
| product | object | 產品信息。 |
| createdAt | number | 對應的秒級時間戳，例如，1672387200。 |
| takerAllocationRate | number | 根據計算時間，估算持有人可從投注池中獲得的比例。到期前，這是估算值。 |
| triggerTime | number | 對於 Rangebound，首次突破時間；對於非 Rangebound，結算時間，單位為秒。 |
| triggerPrice | number | 對於 Rangebound，首次突破價格；對於非 Rangebound，結算價格。 |
| feeRate | object | 費率。 |
| leverageInfo | object | 貸款信息（可選）。 |
| relevantDollarPrices | list[object] | RCH 價格轉換和空投計算所需的代幣價格。 |
| amounts | object | 計算的金額。 |
| apyInfo | object | 年化信息，適用於非 Surge 產品。 |
| oddsInfo | object | 賠率信息，適用於 Surge 產品。 |

**回應範例**

```
{
    "value": [{
        ...
    }]
}
```

### 隱藏虧損持倉

```
POST rfq/position/conceal
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | int | 鏈 ID |
| positionIds | true | List[string] | positionId 列表，最多 20 個 |

**請求範例**

```
{
    "positionIds": [
        "aaaa","bbbb"
    ],
    "chainId": 1
}
```

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 表示返回結果正常   |
| message        | string       | 異常情況下返回的錯誤信息 |
| value          | object       | .                               |


### 獲取待處理交易

這主要是為了解決底層持倉數據同步問題。這裡檢索到的數據可能會消失（由於區塊鏈分叉競爭）。

```
POST rfq/transactions/pending
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 合約地址集合；如果未提供，將查詢所有合約。 |
| taker | false | string | Taker 錢包地址 |
| maker | false | string | Maker 錢包地址 |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 如下所示 |

物件

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | 交易哈希 |
| takerWallet | string | taker 錢包地址。 |
| makerWallet | string | maker 錢包地址。 |
| product | object | 產品信息。 |
| createdAt | number | 對應的秒級時間戳，例如，1672387200。 |
| takerAllocationRate | number | 根據計算時間，估算持有人可從投注池中獲得的比例。到期前，這是估算值。 |
| triggerTime | number | 對於 Rangebound，首次突破時間；對於非 Rangebound，結算時間，單位為秒。 |
| triggerPrice | number | 對於 Rangebound，首次突破價格；對於非 Rangebound，結算價格。 |
| feeRate | object | 費率。 |
| leverageInfo | object | 貸款信息（可選）。 |
| relevantDollarPrices | list[object] | RCH 價格轉換和空投計算所需的代幣價格。 |
| amounts | object | 計算的金額。 |
| apyInfo | object | 年化信息，適用於非 Surge 產品。 |
| oddsInfo | object | 賠率信息，適用於 Surge 產品。 |


### 獲取 RCH 空投歷史

```
GET rfq/airdrop/history
```

**輸入參數**

| **欄位名稱** | **必需** | **類型** | **描述** |
| --- | --- | --- | --- |
| wallet | true | string | 錢包地址|
| startDateTime | true | number | 對應的秒級時間戳（包含），例如，1672387200。 |
| endDateTime | true | number | 對應的秒級時間戳（包含），例如，1672387200。 |
| orderBy | false | string | "dateTime" 或 "rch"，排序方式："dateTime"（空投時間，默認）或 "rch"（金額）。 |
| orderDirection | false | string | "desc" 或 "asc"，默認為 "desc"（降序）。 |

**回應參數**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 如下所示 |

**物件**

| **欄位名稱** | **類型**     | **描述**                                |
| -------------- | ------------ | ---------------------------------------------- |
| dateTime | long | 當前的秒級時間戳，例如，1672387200。 |
| wallet | string | 錢包地址 |
| volume | string | 交易量 |
| rch | string | 空投金額，原始值，需要除以 1e18 |
| merkleProof | string | Merkle 證明  |
