# DApp Open APIs

私たちの既存のdAppであるsofa.orgは、重要なワークフローフィーチャーであり、ユーザーがSOFAプロトコルと対話する主な方法です。しかし、私たちは他の開発者が自分のdAppを通じてSOFAにアクセスし、接続することを奨励し、エコシステムの成長を最大化することを目指しています。これらの開発者を私たちの「ブローカー」パートナーと見なし、興味のある方はSOFAチームにAPIに関するさらなる情報を問い合わせることをお勧めします。

## DNT

### 推奨DNT RFQリスト照会

```
GET /rfq/dnt/recommended-list
```

**入力パラメータ**

| **フィールド名**       | **必須**  | **タイプ** | **説明**                                                                                           |
| -------------------- | ------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| vault              | true          | string   | コントラクト情報                                                                                                   |
| chainId            | true          | int      | チェーンID                                                                                                               |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返された結果が正常であることを示します   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | list[object] | 以下に示す通り                                               |

オブジェクト

| **フィールド名**               | **タイプ**     | **説明**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | チェーンID                                                                                          |
| vault                        | string       | コントラクトアドレス                                                                                  |
| riskType                     | string       | リスクタイプ: PROTECTED, RISKY                                                                       |
| forCcy                       | string       | 基礎通貨                                                                               |
| domCcy                       | string       | 通貨ペア                                                                                     |
| depositCcy                   | string       | サブスクリプション通貨                                                                             |
| lowerBarrier                 | number       | 下限価格                                                                                       |
| upperBarrier                 | number       | 上限価格                                                                                       |
| depositAmount                | number       | RFQ購入額                                                                               |
| expiry                       | number         | 有効期限のタイムスタンプ（例：1672387200）                                                               |
| timestamp                    | number         | 現在の価格のトリガー時間；次の観測開始時間はこのロジックに基づいて計算されます |
| observationStart             | number         | タイムスタンプに基づくノックイン/アウトの観測開始時間の推定値                        |
| feeRate                      | object         | 取引および決済手数料率（オプション）                        |
| leverageInfo                 | object         | ローン情報（オプション）                        |
| relevantDollarPrices         | list[object]   | RCH価格変換およびエアドロップ計算に必要なトークン価格（オプション）                       |
| amounts                      | object         | 計算された金額（オプション）                       |
| apyInfo                     | object         | 年率情報、非サージ製品向けに利用可能（オプション）                     |
| oddsInfo                     | object         | オッズ情報、サージ製品向けに利用可能（オプション）                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral           | string       | メーカーの担保額                                                                         |
| \> totalCollateral           | string       | 総担保額（テイカー+メーカー）                                                             |
| \> collateralAtRisk          | string       | 保証が必要な場合（オプション）                                                           |
| \> makerBalanceThreshold     | string       | メーカーの残高閾値                                                          |
| \> deadline                  | number         | 有効期限のタイムスタンプ（例：1672387200）                                                           |
| \> makerWallet               | string       | メーカーのウォレット（オプション）                                                                     |
| \> signature                 | string       | 署名（オプション）                                                                          |

**リクエスト例**

```
GET rfq/dnt/recommended-list?vault=xxxxxx&chainId=1
```

**レスポンス**

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

### DNT お問い合わせ

- 注意事項:
  - 純粋な問い合わせリクエストでは、ユーザーのウォレットアドレスを渡さないでください。
  - ユーザーのウォレットアドレスは、サブスクリプション時にのみ渡すべきです。

```
GET /rfq/dnt/quote
```

**入力パラメータ**

| **フィールド名**     | **必須** | **タイプ** | **説明**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |

| vault              | true         | string   | 契約情報                                                                                                   |
| chainId            | true         | int      | チェーンID                                                                                                               |
| expiry             | true         | number     | 有効期限の日付の秒単位のタイムスタンプ、例：1672387200                                                           |
| lowerBarrier       | true         | number   | 下限価格                                                                                                            |
| upperBarrier       | true         | number   | 上限価格                                                                                                            |
| depositAmount      | true         | number   | RFQ購入額                                                                                                    |
| inputApyDefinition | true         | string   | 入力APYがどのように計算されるかを示すEnum：OptimusDefaultAPY、BinanceDntAPY、AaveLendingAPY |
| protectedApy       | false        | number   | 保護された年利（RISKYの場合は空、保護された場合は必須）                                                      |
| fundingApy         | false        | number   | AAVE年利（RISKYの場合は空、保護された場合は必須）                                                            |
| takerWallet        | false        | string   | 問い合わせ者のウォレットの公開アドレス情報                                                                           |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返却結果が正常であることを示します   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | object       | 以下に示す通り                                 |

オブジェクト

| **フィールド名**               | **タイプ**     | **説明**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | 契約アドレス                                                                                  |
| chainId                      | int          | チェーンID                                                                                          |
| riskType                     | string       | リスクタイプ：PROTECTED、RISKY                                                                       |
| forCcy                       | string       | 基礎通貨                                                                               |
| domCcy                       | string       | 通貨ペア                                                                                     |
| depositCcy                   | string       | 購入通貨                                                                             |
| lowerBarrier                 | number       | 下限価格                                                                                       |
| upperBarrier                 | number       | 上限価格                                                                                       |
| depositAmount                | number       | RFQ購入額                                                                               |
| expiry                       | number         | 有効期限のタイムスタンプ（例：1672387200）                                                               |
| timestamp                    | number         | 現在の価格のトリガー時間；次の観測開始時間はこのロジックに基づいて計算されます |
| observationStart             | number         | タイムスタンプに基づくノッキングイン/アウトの観測の推定開始時間                        |
| feeRate             | object         | 取引および決済手数料率（オプション）                        |
| leverageInfo             | object         | ローン情報（オプション）                        |
| relevantDollarPrices             | list[object]         | RCH価格変換およびエアドロップ計算に必要なトークン価格（オプション）                       |
| amounts             | object         | 計算された金額（オプション）                       |
| apyInfo             | object         | 年間情報、非サージ製品向けに利用可能（オプション）                     |
| oddsInfo             | object         | オッズ情報、サージ製品向けに利用可能（オプション）                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | メーカーの担保額                                                                         |
| \> totalCollateral            | string       | 合計担保額（テイカー+メーカー）                                                             |
| \> collateralAtRisk | string       | 保証が必要な場合（オプション）                                                           |
| \> makerBalanceThreshold | string       | メーカーの残高閾値                                                          |
| \> deadline                   | number         | 有効期限のタイムスタンプ（例：1672387200）                                                           |
| \> makerWallet                | string       | メーカーのウォレット（オプション）                                                                     |
| \> signature                  | string       | 署名（オプション）                                                                          |

### DNT 勝率

期限までに境界内に留まる確率

```
GET rfq/dnt/winning-probabilities
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | 基礎通貨      |
| expiry          | true         | number     | 有効期限の日付のための第2レベルのタイムスタンプ、例：1672387200 |
| lowerBarrier       | true         | number   | 下限価格 |
| upperBarrier       | true         | number   | 上限価格 |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返される結果が正常であることを示します   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | list[object] | 以下に示す通り                                 |

オブジェクト

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | number | スポット価格 |
| timestamp | number |  |
| probabilities | object | 勝率  |

**リクエスト例**

```
GET rfq/dnt/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerBarrier=xxx&upperBarrier=xxx
```

**レスポンス**

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
            "probBearTrendItmUpperStrike": null
        }
    }
}
```

## スマートトレンド

### 推奨スマートトレンドRFQリスト照会

```
GET rfq/smart-trend/recommended-list
```

**入力パラメータ**

| **フィールド名**       | **必須**  | **タイプ** | **説明**                                         |
| -------------------- | ------------- | -------- | ------------------------------------------------------------ |
| vault              | true          | string   | 契約情報                               |
| chainId            | true          | int      | チェーンID                    |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返却結果が正常であることを示します   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | list[object] | 以下に示す通り                                 |

オブジェクト

| **フィールド名**               | **タイプ**     | **説明**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | チェーンID                                                                                          |
| vault                        | string       | 契約アドレス                                                                                  |
| riskType                     | string       | リスクタイプ: PROTECTED, RISKY                                                                       |
| direction                    | string       | BULLISH, BEARISH |   
| forCcy                       | string       | 基礎通貨                                                                               |
| domCcy                       | string       | 通貨ペア                                                                                     |
| depositCcy                   | string       | 購入通貨                                                                             |
| lowerBarrier                 | number       | 下限価格                                                                                       |
| upperBarrier                 | number       | 上限価格                                                                                       |
| depositAmount                | number       | RFQ購入額                                                                               |
| expiry                       | number         | 有効期限のタイムスタンプ (例: 1672387200)                                                               |
| timestamp                    | number         | 現在の価格のトリガー時間; 次の観測開始時間はこのロジックに基づいて計算されます |
| feeRate                      | object         | 取引および決済手数料率 (オプション)                        |
| leverageInfo                 | object         | ローン情報 (オプション)                        |
| relevantDollarPrices         | list[object]        | RCH価格変換およびエアドロップ計算に必要なトークン価格 (オプション)                       |
| amounts                      | object         | 計算された金額 (オプション)                       |
| apyInfo                      | object         | 年率情報、サージ製品以外で利用可能 (オプション)                     |
| oddsInfo                     | object         | オッズ情報、サージ製品で利用可能 (オプション)                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | メーカーの担保額                                                                         |
| \> totalCollateral            | string       | 総担保額 (テイカー+メーカー)                                                             |
| \> collateralAtRisk | string       | 保証が必要な場合 (オプション)                                                           |
| \> makerBalanceThreshold | string       | メーカーの残高閾値                                                          |
| \> deadline                   | number         | 有効期限のタイムスタンプ (例: 1672387200)                                                           |
| \> makerWallet                | string       | メーカーのウォレット (オプション)                                                                     |
| \> signature                  | string       | 署名 (オプション)                                                                          |

**リクエスト例**

```
GET rfq/smart-trend/recommended-list?vault=xxxxxx
```

### スマートトレンド照会

- 注意事項:
  - 純粋な照会リクエストではユーザーのウォレットアドレスを渡さないでください。
  - ユーザーのウォレットアドレスは、サブスクリプション時のみ渡すべきです。

```
GET /rfq/smart-trend/quote
```

**入力パラメータ**

| **フィールド名**     | **必須** | **タイプ** | **説明**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | コントラクト情報                                                                                                   |
| chainId            | true         | int      | チェーンID                                                                                                               |
| expiry             | true         | number     | 有効期限の日付の秒単位タイムスタンプ (例: 1672387200)                                                           |
| lowerBarrier       | true         | number   | 下限価格                                                                                                            |
| upperBarrier       | true         | number   | 上限価格                                                                                                            |
| depositAmount      | true         | number   | RFQ購入額                                                                                                    |
| inputApyDefinition | true         | string   | 入力APYがどのように計算されるかを示すEnum: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | 保証された年利 (RISKYの場合は空、保護された場合は必須)                                                      |
| fundingApy         | false        | number   | AAVE年利 (RISKYの場合は空、保護された場合は必須)                                                            |
| takerWallet        | false        | string   | 問い合わせ者のウォレットの公開アドレス情報                                                                           |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返却結果が正常であることを示します   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | object       | 以下に示す通り                                 |

オブジェクト

| **フィールド名**               | **タイプ**     | **説明**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | 契約アドレス                                                                                  |
| chainId                      | int          | チェーンID                                                                                          |
| riskType                     | string       | リスクタイプ: PROTECTED, RISKY                                                                       |
| direction                    | string       | BULLISH, BEARISH | 
| forCcy                       | string       | 基礎通貨                                                                               |
| domCcy                       | string       | 通貨ペア                                                                                     |
| depositCcy                   | string       | 購入通貨                                                                             |
| lowerBarrier                 | number       | 下限価格                                                                                       |
| upperBarrier                 | number       | 上限価格                                                                                       |
| depositAmount                | number       | RFQ購入金額                                                                               |
| expiry                       | number         | 有効期限のタイムスタンプ (例: 1672387200)                                                               |
| timestamp                    | number         | 現在の価格のトリガー時間; 次の観測開始時間はこのロジックに基づいて計算されます |
| feeRate                      | object         | 取引および決済手数料率 (オプション)                        |
| leverageInfo                 | object         | ローン情報 (オプション)                        |
| relevantDollarPrices         | list[object]         | RCH価格変換およびエアドロップ計算に必要なトークン価格 (オプション)                       |
| amounts                      | object         | 計算された金額 (オプション)                       |
| apyInfo                      | object         | 年率情報、非サージ製品向け (オプション)                     |
| oddsInfo                     | object         | オッズ情報、サージ製品向け (オプション)                    |
| quote                        | object       |                                                                                                   |
| \> quoteId                  | number |                                                                           |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral           | string       | メイカーの担保額                                                                         |
| \> totalCollateral           | string       | 総担保額 (テイカー+メイカー)                                                             |
| \> collateralAtRisk          | string       | 保証が必要な場合 (オプション)                                                           |
| \> makerBalanceThreshold     | string       | メイカーのバランス閾値                                                          |
| \> deadline                  | number         | 有効期限のタイムスタンプ (例: 1672387200)                                                           |
| \> makerWallet               | string       | メイカーのウォレット (オプション)                                                                     |
| \> signature                 | string       | 署名 (オプション)                                                                          |

### スマートトレンド勝率

期限までに境界内に留まる確率

```
GET rfq/smart-trend/winning-probabilities
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | 基礎通貨      |
| expiry          | true         | number     | 期限日のための第2レベルのタイムスタンプ、例：1672387200 |
| lowerBarrier       | true         | number   | 下限価格 |
| upperBarrier       | true         | number   | 上限価格 |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返却結果が正常であることを示す   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | list[object] | 以下に示す通り                                 |

オブジェクト

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | number | 現物価格 |
| timestamp | number |  |
| probabilities | object | 勝率  |


**リクエスト例**

```
GET rfq/smart-trend/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerStrike=xxx&upperStrike=xxx
```

**レスポンス**

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

## 一般インターフェース

### RFQの削除

オンチェーンに存在するRFQを削除します。

```
POST rfq/remove
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| rfqId          | true         | number       | .       |

**リクエスト例**

```
{  
"rfqId":123456  
}
```

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返却結果が正常であることを示します   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | object       | .                               |


### 取引通知

```
POST rfq/trade
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| quotes | true | list[object] |   |
| \> rfqId | true | number |     |
| \> quoteId | true | number |     |
| \> txId | true | string | 取引ハッシュ |
| code | false | string | 招待コード |
| walletType | false | string | MetaMask、OKX Wallet、Coinbaseなどのウォレットタイプ |

**リクエスト例**

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

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返却結果が正常であることを示します   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | object       | .                               |

### ストライクリスト

```
GET rfq/strike-list
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| indexPrice | true | number |  |
| forCcy | false | string | BTC-USDT取引ペアの場合、ここにBTCを入力してください。指定がない場合は、デフォルト設定が使用されます。 |
| domCcy | false | string | BTC-USDT取引ペアの場合、ここにUSDTを入力してください。指定がない場合は、デフォルトでUSDTになります。 |


**リクエスト例**

```
{  
"indexPrice": 3750.8，  
"forCcy": "WBTC",  
}
```

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| strikes | list[number] | デフォルトの推奨ストライク価格リスト |

### 有効期限リスト

```
GET rfq/expiry-list
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| vault | true | string | コントラクトアドレス |
| chainId | true | int | チェーンID |


**リクエスト例**

```
{  
"vault": "XXXXXXXXXXXXXX",  
"chainId": 1,  
}
```

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| timestamp | number | ベースライン開始時間（秒単位のタイムスタンプ） |
| expiries | list[number] | サポートされている有効期限リスト（秒単位のタイムスタンプ）、例：1672387200 |

### Aave apy

```
GET rfq/aave-apy
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | チェーンID |
| ccy | string | 入金通貨 |
| avgApy | string | 過去30日間にオンチェーンで記録された平均APY |
| currentApy | string | 最新のAAVE APY |
| apyUsed | string | 将来の利息収入を推定するためにSofaServerで実際に使用されたAPY |
| apyDefinition | string | APYに対応する計算定義 |

**レスポンス例**

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

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |
| apyDefinition | false | string | APYに対応する計算定義; デフォルトはAaveLendingApy |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | チェーンID |
| ccy | string | 入金通貨 |
| avgApy | string | 過去30日間にオンチェーンで記録された平均APY |
| currentApy | string | 最新のAPY |
| apyUsed | string | 将来の利息収入を見積もるためにSofaServerで実際に使用されたAPY |
| apyDefinition | string | APYに対応する計算定義 |

**レスポンス例**

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

### ウォレットポジションを取得

```
POST /rfq/position-list
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | コントラクトアドレスのセット; 提供されていない場合、すべてのコントラクトがクエリされます。 |
| claimed | false | boolean | 引き換えられたかどうか; 提供されていない場合、すべてのステータスのポジションがクエリされます。 |
| expired | false | boolean | 期限切れかどうか; 提供されていない場合、すべてのステータスのポジションがクエリされます。 |
| concealed | false | boolean | 隠されているかどうか; 提供されていない場合、すべてのステータスのポジションがクエリされます。 |
| positiveReturn | false | boolean | 引き換え金額が0より大きいかどうか; 提供されていない場合、すべてのポジションがクエリされます。 |
| positiveProfit | false | boolean | リターンが元本を超えているかどうか; 提供されていない場合、すべてのポジションがクエリされます。 |
| limit | false | Int | クエリの数; デフォルトは100、最大は300です。 |
| startDateTime | false | number | 対応するタイムスタンプ（秒単位、含む）、例: 1672387200。 |
| endDateTime | false | number | 対応するタイムスタンプ（秒単位、含む）、例: 1672387200。 |
| orderBy | false | string | "createdAt" または "return"、ソート方法: "createdAt"（更新時間、デフォルト）または "return"（リターン）。 |
| orderDirection | false | string | "desc" または "asc"、デフォルトは "desc"（降順）。 |
| wallet | false | string | ウォレットアドレス（空の場合、すべてのウォレットアドレスがクエリされます）。 |

注: 指定された `startDateTime` と `endDateTime` の範囲内で結果の数が300を超える場合、ポーリングを使用できます（この場合、`orderBy` は `"createdAt"` に設定する必要があります）。n回目のポーリングリクエストの `endDateTime` パラメータは、(n-1)回目のポーリング結果の最後のレコードの `createdAt` に設定する必要があります。すべてのデータを取得した後、`id` フィールドに基づいて重複を削除する必要があります。

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 以下のように |

オブジェクト

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string | ポジションに対応する製品ID（The Graphデータの `productId` に一致）。このIDはチェーン + ボールトの次元内でユニークであり、ポジションの残高をクエリするために使用できます。 |
| positionId | string | ポジションIDで、チェーン次元内でユニークです。 |
| product | object | 製品情報。 |
| wallet | string | ウォレットアドレス。 |
| createdAt | number | 対応するタイムスタンプ（秒単位）、例: 1672387200。 |
| updatedAt | number | 対応するタイムスタンプ（秒単位）、例: 1672387200。 |
| claimed | boolean | ポジションが引き換えられたかどうか。 |
| takerAllocationRate | number | 計算時点に基づき、オーナーがベッティングプールから取ることができる割合を推定します。期限切れ前は、これは推定値です。 |
| triggerTime | number | Rangeboundの場合、最初のブレイクアウト時間; 非Rangeboundの場合、決済時間（秒単位）。 |
| triggerPrice | number | Rangeboundの場合、最初のブレイクアウト価格; 非Rangeboundの場合、決済価格。 |
| feeRate | object | 手数料率。 |
| leverageInfo | object | ローン情報（オプション）。 |
| relevantDollarPrices | list[object] | RCH価格変換およびエアドロップ計算に必要なトークン価格。 |
| amounts | object | 計算された金額。 |
| apyInfo | object | 年率情報、非サージ製品向けに利用可能。 |
| oddsInfo | object | オッズ情報、サージ製品向けに利用可能。 |
| claimParams | object | クレームパラメータ情報。 |

**レスポンス例**

```
{
    "value": [{
        ...
    }]
}
```

### ウォレット取引の取得

```
POST /rfq/transaction-list
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | コントラクトアドレスのセット；指定しない場合は、すべてのコントラクトがクエリされます。 |
| limit | false | Int | クエリの数；デフォルトは100、最大は300です。 |
| startDateTime | false | number | 対応するタイムスタンプ（秒単位、含む）、例：1672387200。 |
| endDateTime | false | number | 対応するタイムスタンプ（秒単位、含む）、例：1672387200。 |
| orderDirection | false | string | "desc"または"asc"、デフォルトは"desc"（降順）。 |
| taker | false | string | テイカーのウォレットアドレス |
| maker | false | string | メイカーのウォレットアドレス |
| claimParams | false | object | 引き換えパラメータ、ポジションリストデータのclaimParamsフィールドに対応。 |
| hash | false | string | トランザクションハッシュ |

注：テイカー、メイカー、およびclaimParamsパラメータは、ポジションに対応する取引記録を見つけるための共同クエリに使用できます（ポジションは複数の取引に対応する場合があります）（すべてのパラメータは必須ではありません）。指定された`startDateTime`と`endDateTime`内で結果の数が300を超える場合、ポーリングを使用してクエリを続行できます（この場合、`orderBy`は`"createdAt"`に設定する必要があります）。n回目のポーリングリクエストの`endDateTime`パラメータは、（n-1）回目のポーリング結果の最後のレコードの`createdAt`に設定する必要があります。すべてのデータを取得した後、`id`フィールドに基づいて重複を削除する必要があります。

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 以下に示す通り |

オブジェクト

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | トランザクションハッシュ |
| takerWallet | string | テイカーウォレットアドレス。 |
| makerWallet | string | メイカーウォレットアドレス。 |
| product | object | 製品情報。 |
| createdAt | number | 対応するタイムスタンプ（秒単位）、例：1672387200。 |
| takerAllocationRate | number | 計算時点に基づき、オーナーがベッティングプールから取れる割合を推定します。期限前は、これは推定値です。 |
| triggerTime | number | レンジバウンドの場合、最初のブレイクアウト時間；非レンジバウンドの場合、決済時間（秒単位）。 |
| triggerPrice | number | レンジバウンドの場合、最初のブレイクアウト価格；非レンジバウンドの場合、決済価格。 |
| feeRate | object | 手数料率。 |
| leverageInfo | object | ローン情報（オプション）。 |
| relevantDollarPrices | list[object] | RCH価格変換およびエアドロップ計算に必要なトークン価格。 |
| amounts | object | 計算された金額。 |
| apyInfo | object | 年率情報、非サージ製品に利用可能。 |
| oddsInfo | object | オッズ情報、サージ製品に利用可能。 |

**レスポンス例**

```
{
    "value": [{
        ...
    }]
}
```

### 損失ポジションを隠す

```
POST rfq/position/conceal
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| chainId | true | int | チェーンID |
| positionIds | true | List[string] | positionIdリスト、最大20 |

**リクエスト例**

```
{
    "positionIds": [
        "aaaa","bbbb"
    ],
    "chainId": 1
}
```

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0は返される結果が正常であることを示す   |
| message        | string       | 例外が発生した場合に返されるエラーメッセージ |
| value          | object       | .                               |


### 保留中のトランザクションを取得

これは主に基盤となるポジションデータの同期問題に対処するためのものです。ここで取得されたデータは消失する可能性があります（ブロックチェーンのフォーク競争による）。

```
POST rfq/transactions/pending
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | コントラクトアドレスのセット; 提供されない場合、すべてのコントラクトがクエリされます。 |
| taker | false | string | テイカーのウォレットアドレス |
| maker | false | string | メイカーのウォレットアドレス |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 以下のように示されます |

オブジェクト

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | トランザクションハッシュ |
| takerWallet | string | テイカーのウォレットアドレス。 |
| makerWallet | string | メイカーのウォレットアドレス。 |
| product | object | 製品情報。 |
| createdAt | number | 秒単位の対応するタイムスタンプ、例: 1672387200。 |
| takerAllocationRate | number | 計算時点に基づき、オーナーがベッティングプールから取得できる割合を推定します。期限前は、これは推定値です。 |
| triggerTime | number | レンジバウンドの場合、最初のブレイクアウト時間; 非レンジバウンドの場合、決済時間（秒単位）。 |
| triggerPrice | number | レンジバウンドの場合、最初のブレイクアウト価格; 非レンジバウンドの場合、決済価格。 |
| feeRate | object | 手数料率。 |
| leverageInfo | object | ローン情報（オプション）。 |
| relevantDollarPrices | list[object] | RCH価格変換およびエアドロップ計算に必要なトークン価格。 |
| amounts | object | 計算された金額。 |
| apyInfo | object | 年率情報、非サージ製品に利用可能。 |
| oddsInfo | object | オッズ情報、サージ製品に利用可能。 |

### RCHエアドロップ履歴を取得する

```
GET rfq/airdrop/history
```

**入力パラメータ**

| **フィールド名** | **必須** | **タイプ** | **説明** |
| --- | --- | --- | --- |
| wallet | true | string | ウォレットアドレス |
| startDateTime | true | number | 対応するタイムスタンプ（秒単位、含む）、例：1672387200。 |
| endDateTime | true | number | 対応するタイムスタンプ（秒単位、含む）、例：1672387200。 |
| orderBy | false | string | "dateTime" または "rch"、ソート方法："dateTime"（エアドロップ時間、デフォルト）または "rch"（金額）。 |
| orderDirection | false | string | "desc" または "asc"、デフォルトは "desc"（降順）。 |

**レスポンスパラメータ**

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 以下のように示されます |

オブジェクト

| **フィールド名** | **タイプ**     | **説明**                                |
| -------------- | ------------ | ---------------------------------------------- |
| dateTime | long | 現在のタイムスタンプ（秒単位）、例：1672387200。 |
| wallet | string | ウォレットアドレス |
| volume | string | 取引量 |
| rch | string | エアドロップ金額、生の値であり、1e18で割る必要があります |
| merkleProof | string | メルクル証明 |
