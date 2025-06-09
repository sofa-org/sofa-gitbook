# マーケットメイカーのオープンAPI

## 仕様

### プレフィックス

1. 取引の安全性を確保するため、通信にはHTTPSを使用してください。
2. データ交換フォーマットはJSONです。
3. UTF-8文字エンコーディングが普遍的に適用されます。
4. インターフェース署名アルゴリズムはHMAC-SHA256を利用します。
5. UNIXミリ秒タイムスタンプが使用され、1970年1月1日0:00:00からのミリ秒数を表します。

### パラメータ

#### リクエスト

| name          | type   | remark                                                  |
| ------------- | ------ | ------------------------------------------------------- |
| H-Request-Id  | string | [ヘッダー]リクエストID、ユニーク                       |
| H-Api-Key     | string | [ヘッダー]APIキー                                       |
| H-Timestamp   | long   | [ヘッダー]有効なタイムスタンプ、例：1672387200000      |
| H-Nonce       | string | [ヘッダー]ランダム文字列                               |
| Authorization | string | [ヘッダー]署名、例：[mm_id]-hmac-sha256署名           |

#### レスポンス

| name    | type    | ramark             |
| ------- | ------- | ------------------ |
| code    | integer | [ボディ]エラーコード |
| message | string  | [ボディ]エラー理由   |
| value   | T       | [ボディ]結果         |

### 署名生成

当社のRFQプラットフォームでは、リクエストを承認するためにパートナー署名が必要で、その後に検証手続きが行われます。 検証に失敗すると、プラットフォームは`401` Unauthorizedレスポンスで拒否します。

#### 署名文字列の構築:

署名文字列は5行で構成され、それぞれがパラメータを表します。 各行はセミコロンで終わり、最後の行も含まれます。有効なタイムスタンプとリクエストノンスは、それぞれヘッダーの`H-Timestamp`と`H-Nonce`パラメータから取得されます。

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### 署名の生成

`SecretKey`を使用して`StringToSign`を`HMAC-SHA256`で暗号化します。

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### HTTPヘッダーの設定

リクエストは`HTTP Authorization`ヘッダーを通じて署名を送信します。`Authorization header`は二つの部分から構成されています: **認証タイプ**と**署名情報**。

```
Authorization: AuthenticationType SignatureInformation
```

1. 認証タイプ: `[mm_id]-hmac-sha256`
2. 署名情報: `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_注意:_

- リクエストメソッドは大文字である必要があります。
- 署名に使用される`RequestBody`はリクエストのボディの内容と一致しなければなりません。
- GETおよびDELETEリクエストの場合、`URI`にはリクエストパラメータを含める必要があります（例: /api/v1/result?orderId=123）。
  - リクエストボディがない場合（GETリクエストに一般的）、リクエストボディは空の文字列("")である必要があります。
- 有効なタイムスタンプ（H-Timestamp）はリクエスターによって決定されます。有効なタイムスタンプを超えるリクエストはRFQサーバーによって拒否されます。

## RFQ

### DNT RFQ見積もりの提供

```
GET rfq/dnt/quote
```

**パラメータ**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | ボールトアドレス                                                 |
| chainId                 | true     | int    | チェーンID                                                        |
| expiry                  | true     | long   | 有効期限（秒単位のタイムスタンプ）、例：1672387200              |
| lowerBarrier            | true     | number | 下限バリア                                                       |
| upperBarrier            | true     | number | 上限バリア                                                       |
| depositAmount           | true     | number | デポジット                                                       |
| premiumAmount           | true     | number | プレミアム                                                       |
| protectedFundingAmount  | false    | number | 保護された利息（RISKYの場合はnull）                             |
| deadline                | true     | long   | 見積もりの締切、例：1672387200                                    |
| takerWallet             | false    | string | テイカーウォレットアドレス                                      |
| anchorPricesDecimal     | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| collateralAtRiskDecimal | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | 基本ペア、例：BTC-USDT                                           |
| trackingSource          | true     | string | 基本価値を追跡するために使用されるデータソース、例：DERIBIT   |
| depositCoin             | true     | string | DNTを購読するために支払われるプレミアムの通貨/コイン          |
| tradingFeeRate          | true     | number |                                                                   |
| settlementFeeRate       | true     | number |                                                                   |
| riskType                | true     | string | タイプ：PROTECTED, RISKY                                        |

**レスポンス**

| name                       | type         | description                        |
| -------------------------- | ------------ | ---------------------------------- |
| timestamp                  | long         | 見積もりのタイムスタンプ          |
| vault                      | string       |                                    |
| chainId                    | int          |                                    |
| expiry                     | long         | 有効期限のタイムスタンプ、例：1672387200 |
| anchorPrices               | list[string] | 20000000000,30000000000            |
| makerCollateral            | string       |                                    |
| totalCollateral            | string       |                                    |
| collateralAtRisk           | string       | E18                                |
| makerBalanceThreshold      | string       |                                    |
| deadline                   | long         | 見積もりの締切タイムスタンプ       |
| makerWallet                | string       |                                    |
| signature                  | string       | .                                   |

Note:

1. $$collateralAtRisk - makerCollateral ==  premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**例**

リクエストURL

```
rfq/dnt/quote
```

パラメータ

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
### ブル/ベアトレンドRFQ見積もりを提供する

```

GET rfq/smart-trend/quote
```

**パラメータ**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | ボールトアドレス                                                  |
| chainId                 | true     | int    | チェーンID                                                        |
| expiry                  | true     | long   | 有効期限（秒単位のタイムスタンプ）、例：1672387200              |
| direction               | true     | string | 強気 / 弱気                                                      |
| lowerStrike             | true     | number | 下限バリア                                                      |
| upperStrike             | true     | number | 上限バリア                                                      |
| depositAmount           | true     | number | デポジット                                                      |
| premiumAmount           | true     | number | プレミアム                                                      |
| protectedFundingAmount  | false    | number | 保護された利息（リスキーな場合はnull）                          |
| deadline                | true     | long   | 見積もりの締切、例：1672387200                                   |
| takerWallet             | false    | string | テイカーウォレットアドレス                                      |
| anchorPricesDecimal     | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| collateralAtRiskDecimal | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | 基礎ペア、例：BTC-USDT                                           |
| trackingSource          | true     | string | 基礎価値を追跡するために使用されるデータソース、例：DERIBIT   |
| tradingFeeRate          | true     | number |                                                                   |
| settlementFeeRate       | true     | number |                                                                   |
| depositCoin             | true     | string | スマートトレンドにサブスクライブするために支払われるプレミアムの通貨/コイン |
| riskType                | true     | string | タイプ：保護、リスキー                                          |

**レスポンス**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | 見積もりのタイムスタンプ          |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | 有効期限のタイムスタンプ、例：1672387200 |
| anchorPrices     | list[string] | 20000000000,30000000000            |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |

| collateralAtRisk | string       | E18                                |
| deadline         | long         | 見積もりの締切タイムスタンプ           |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

Note:

- $$PremiumAmount = BookingQuantity * UnitQuotePrice$$
- $$MaxPayoutAmount = BookingQuantity * (upperStrike - lowerStrike)$$
- $$MinAPYAmount == projectedFundingAmount - premiumAmount$$
- $$MaxAPYAmount == MinAPYAmount + MaxPayoutAmount$$
- $$makerCollateral = MaxPayoutAmount - premiumAmount$$
- $$collateralAtRisk = MaxPayoutAmount$$
- $$totalCollateral = depositAmount + makerCollateral$$
- $$anchorPrices = [lowerStrike, upperStrike]$$

### デュアルRFQ見積もりを提供する

```
GET rfq/dual/quote
```

**パラメータ**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | ボールトアドレス                                                 |
| chainId                 | true     | int    | チェーンID                                                        |
| expiry                  | true     | long   | 有効期限（秒単位のタイムスタンプ）、例：1672387200                |
| strike                  | true     | number | ストライク価格                                                    |
| type                    | true     | string | CALLまたはPUT                                                     |
| depositAmount           | true     | number | デポジット                                                       |
| deadline                | true     | long   | 見積もりの締切、例：1672387200                                    |
| refDateTime             | true     | long   | 現在のリクエスト時間                                            |
| takerWallet             | false    | string | テイカーウォレットアドレス                                      |
| anchorPriceDecimal      | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | アンダーライングペア、例：BTC-USDT                               |
| trackingSource          | true     | string | アンダーライングの価値を追跡するために使用されるデータソース、例：DERIBIT |
| depositCoin             | true     | string | デュアルに加入するために支払われたプレミアムの通貨/コイン             |
| depositCoinTokenAddress | true     | string |                                                                   |
| depositCoinTokenDecimal | true     | long   |                                                                   |
| tradingFeeRate          | true     | number | .                                                                  |

**Response**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | 見積もりのタイムスタンプ                    |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | 有効期限のタイムスタンプ，例：1672387200 |
| anchorPrice      | string       | 20000000000                        |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| deadline         | long         | 見積もりの締切タイムスタンプ           |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

## Appendix

| Code | Message                                                          |
| ---- | ---------------------------------------------------------------- |
| 1000 | システムエラー。                                                    |
| 2001 | サインエラー。                                                      |
| 2002 | パラメーターエラー。                                                     |
| 3001 | 要求された情報は存在しません。                            |
| 3002 | 入金額が _depositRange_ の範囲外です。                        |
| 3003 | 最大加入制限に達しました。                          |
| 3004 | プレミアム額の差が大きすぎて加入に失敗しました。 |
| 3005 | 見積もりに失敗しました。                                                    |
| 3006 | 一時的にサービスを提供していません。                              |
| 3007 | APIのレート制限を超えました。スローダウンしてください。                          |
| 3100 | 注文の作成に失敗しました。                                           |

**Required Functions**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$
