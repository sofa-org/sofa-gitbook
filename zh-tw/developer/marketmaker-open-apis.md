# 市場做市商開放 API

## 規範

### 前綴

1. 為確保交易安全，使用 HTTPS 傳輸。
2. JSON 是數據交換格式。
3. 普遍使用 UTF-8 字符編碼。
4. 接口簽名算法使用 HMAC-SHA256。
5. 使用 UNIX 毫秒時間戳，表示自 1970 年 1 月 1 日 0:00:00 起的毫秒數。

### 參數

#### 請求

| 名稱          | 類型   | 備註                                                  |
| ------------- | ------ | ----------------------------------------------------- |
| H-Request-Id  | string | [Header]請求 ID，唯一                                 |
| H-Api-Key     | string | [Header]API Key                                       |
| H-Timestamp   | long   | [Header]有效時間戳，例如：1672387200000               |
| H-Nonce       | string | [Header]隨機字符串                                    |
| Authorization | string | [Header]簽名，例如：[mm_id]-hmac-sha256 簽名          |

#### 響應

| 名稱    | 類型    | 備註             |
| ------- | ------- | ---------------- |
| code    | integer | [Body]錯誤代碼   |
| message | string  | [Body]錯誤原因   |
| value   | T       | [Body]結果       |

### 簽名生成

我們的 RFQ 平台要求合作夥伴簽名以批准請求，隨後進行驗證程序。驗證失敗將導致平台拒絕，並返回 `401` 未授權響應。

#### 構建簽名字符串：

簽名字符串由五行組成，每行代表一個參數。每行以分號結束，包括最後一行。有效時間戳和請求隨機數分別取自標頭中的 `H-Timestamp` 和 `H-Nonce` 參數。

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### 填充簽名

使用 `SecretKey` 來加密 `StringToSign`，使用 `HMAC-SHA256`。

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### 設置 HTTP 標頭

請求通過 `HTTP Authorization` 標頭傳輸簽名。`Authorization header` 由兩部分組成：**認證類型** 和 **簽名信息**。

```
Authorization: AuthenticationType SignatureInformation
```

1. 認證類型: `[mm_id]-hmac-sha256`
2. 簽名信息: `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_注意:_

- 請求方法應為大寫。
- 用於簽名的 `RequestBody` 必須與請求的正文內容匹配。
- 對於 GET 和 DELETE 請求，`URI` 應包括請求參數（例如，/api/v1/result?orderId=123）。
  - 如果沒有請求正文（通常對於 GET 請求），請求正文應為空字符串 ("")。
- 有效時間戳（H-Timestamp）由請求者確定；超出有效時間戳的請求將被 RFQ 服務器拒絕。

## RFQ

### 提供 DNT RFQ 報價

```
GET rfq/dnt/quote
```

**參數**

| 名稱                    | 必需   | 類型   | 描述                                                             |
| ----------------------- | ------ | ------ | ---------------------------------------------------------------- |
| vault                   | true   | string | 保管庫地址                                                       |
| chainId                 | true   | int    | 鏈 ID                                                            |
| expiry                  | true   | long   | 到期時間，以秒為單位的時間戳，例如：1672387200                   |
| lowerBarrier            | true   | number | 下限                                                             |
| upperBarrier            | true   | number | 上限                                                             |
| depositAmount           | true   | number | 存款                                                             |
| premiumAmount           | true   | number | 保費                                                             |
| protectedFundingAmount  | false  | number | 受保利息（RISKY 時為 null）                                      |
| deadline                | true   | long   | 報價截止時間，例如：1672387200                                   |
| takerWallet             | false  | string | 承接者錢包地址                                                  |
| anchorPricesDecimal     | true   | long   |                                                                 |
| makerCollateralDecimal  | true   | long   |                                                                 |
| collateralAtRiskDecimal | true   | long   |                                                                 |
| totalCollateralDecimal  | true   | long   |                                                                 |
| underlyingPair          | true   | string | 基礎對，例如 BTC-USDT                                           |
| trackingSource          | true   | string | 用於跟踪基礎價值的數據來源，例如 DERIBIT                         |
| depositCoin             | true   | string | 支付保費以訂閱 DNT 的貨幣/幣種                                   |
| tradingFeeRate          | true   | number |                                                                 |
| settlementFeeRate       | true   | number |                                                                 |
| riskType                | true   | string | 類型：PROTECTED, RISKY                                          |

**響應**

| 名稱                       | 類型         | 描述                             |
| -------------------------- | ------------ | -------------------------------- |
| timestamp                  | long         | 報價時間戳                       |
| vault                      | string       |                                  |
| chainId                    | int          |                                  |
| expiry                     | long         | 到期時間戳，例如：1672387200     |
| anchorPrices               | list[string] | 20000000000,30000000000          |
| makerCollateral            | string       |                                  |
| totalCollateral            | string       |                                  |
| collateralAtRisk           | string       | E18                              |
| makerBalanceThreshold      | string       |                                  |
| deadline                   | long         | 報價截止時間戳                   |
| makerWallet                | string       |                                  |
| signature                  | string       | .                                |

注意：

1. $$collateralAtRisk - makerCollateral ==  premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**示例**

請求 URL

```
rfq/dnt/quote
```

參數

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
### 提供牛/熊趨勢 RFQ 報價

```
GET rfq/smart-trend/quote
```

**參數**

| 名稱                    | 必需   | 類型   | 描述                                                             |
| ----------------------- | ------ | ------ | ---------------------------------------------------------------- |
| vault                   | true   | string | 保管庫地址                                                       |
| chainId                 | true   | int    | 鏈 ID                                                            |
| expiry                  | true   | long   | 到期時間，以秒為單位的時間戳，例如：1672387200                   |
| direction               | true   | string | BULLISH / BEARISH                                               |
| lowerStrike             | true   | number | 下限                                                             |
| upperStrike             | true   | number | 上限                                                             |
| depositAmount           | true   | number | 存款                                                             |
| premiumAmount           | true   | number | 保費                                                             |
| protectedFundingAmount  | false  | number | 受保利息（RISKY 時為 null）                                      |
| deadline                | true   | long   | 報價截止時間，例如：1672387200                                   |
| takerWallet             | false  | string | 承接者錢包地址                                                  |
| anchorPricesDecimal     | true   | long   |                                                                 |
| makerCollateralDecimal  | true   | long   |                                                                 |
| collateralAtRiskDecimal | true   | long   |                                                                 |
| totalCollateralDecimal  | true   | long   |                                                                 |
| underlyingPair          | true   | string | 基礎對，例如 BTC-USDT                                           |
| trackingSource          | true   | string | 用於跟踪基礎價值的數據來源，例如 DERIBIT                         |
| tradingFeeRate          | true   | number |                                                                 |
| settlementFeeRate       | true   | number |                                                                 |
| depositCoin             | true   | string | 支付保費以訂閱 Smart Trend 的貨幣/幣種                           |
| riskType                | true   | string | 類型：PROTECTED, RISKY                                          |

**響應**

| 名稱             | 類型         | 描述                             |
| ---------------- | ------------ | -------------------------------- |
| timestamp        | long         | 報價時間戳                       |
| vault            | string       |                                  |
| chainId          | int          |                                  |
| expiry           | long         | 到期時間戳，例如：1672387200     |
| anchorPrices     | list[string] | 20000000000,30000000000          |
| makerCollateral  | string       |                                  |
| totalCollateral  | string       |                                  |
| collateralAtRisk | string       | E18                              |
| deadline         | long         | 報價截止時間戳                   |
| makerWallet      | string       |                                  |
| signature        | string       | .                                |

注意：

- $$PremiumAmount = BookingQuantity * UnitQuotePrice$$
- $$MaxPayoutAmount = BookingQuantity * (upperStrike - lowerStrike)$$
- $$MinAPYAmount == projectedFundingAmount - premiumAmount$$
- $$MaxAPYAmount == MinAPYAmount + MaxPayoutAmount$$
- $$makerCollateral = MaxPayoutAmount - premiumAmount$$
- $$collateralAtRisk = MaxPayoutAmount$$
- $$totalCollateral = depositAmount + makerCollateral$$
- $$anchorPrices = [lowerStrike, upperStrike]$$

### 提供雙重 RFQ 報價

```
GET rfq/dual/quote
```

**參數**

| 名稱                    | 必需   | 類型   | 描述                                                             |
| ----------------------- | ------ | ------ | ---------------------------------------------------------------- |
| vault                   | true   | string | 保管庫地址                                                       |
| chainId                 | true   | int    | 鏈 ID                                                            |
| expiry                  | true   | long   | 到期時間，以秒為單位的時間戳，例如：1672387200                   |
| strike                  | true   | number | 行權價                                                          |
| type                    | true   | string | CALL 或 PUT                                                     |
| depositAmount           | true   | number | 存款                                                             |
| deadline                | true   | long   | 報價截止時間，例如：1672387200                                   |
| refDateTime             | true   | long   | 當前請求時間                                                    |
| takerWallet             | false  | string | 承接者錢包地址                                                  |
| anchorPriceDecimal      | true   | long   |                                                                 |
| makerCollateralDecimal  | true   | long   |                                                                 |
| totalCollateralDecimal  | true   | long   |                                                                 |
| underlyingPair          | true   | string | 基礎對，例如 BTC-USDT                                           |
| trackingSource          | true   | string | 用於跟踪基礎價值的數據來源，例如 DERIBIT                         |
| depositCoin             | true   | string | 支付保費以訂閱 Dual 的貨幣/幣種                                 |
| depositCoinTokenAddress | true   | string |                                                                 |
| depositCoinTokenDecimal | true   | long   |                                                                 |
| tradingFeeRate          | true   | number | .                                                               |

**響應**

| 名稱             | 類型         | 描述                             |
| ---------------- | ------------ | -------------------------------- |
| timestamp        | long         | 報價時間戳                       |
| vault            | string       |                                  |
| chainId          | int          |                                  |
| expiry           | long         | 到期時間戳，例如：1672387200     |
| anchorPrice      | string       | 20000000000                      |
| makerCollateral  | string       |                                  |
| totalCollateral  | string       |                                  |
| deadline         | long         | 報價截止時間戳                   |
| makerWallet      | string       |                                  |
| signature        | string       | .                                |

## 附錄

| 代碼 | 信息                                                             |
| ---- | ---------------------------------------------------------------- |
| 1000 | 系統錯誤。                                                       |
| 2001 | 簽名錯誤。                                                       |
| 2002 | 參數錯誤。                                                       |
| 3001 | 請求的信息不存在。                                               |
| 3002 | 存款金額超出 _depositRange_。                                    |
| 3003 | 已達到最大訂閱限制。                                             |
| 3004 | 由於保費金額差異過大，訂閱失敗。                                 |
| 3005 | 報價失敗。                                                       |
| 3006 | 暫時不提供服務。                                                 |
| 3007 | 超過 API 速率限制。請放慢速度。                                 |
| 3100 | 訂單創建失敗。                                                   |

**所需功能**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$