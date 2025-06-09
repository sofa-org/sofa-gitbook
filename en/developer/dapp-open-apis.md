# DApp Open APIs

Our existing dApp on sofa.org is a critical workflow feature and is the main way for users to interact with the SOFA protocols.  However, we encourage other developers to enable access and connect to SOFA via their own dApps as well to maximize our ecosystem growth.  We consider these to be our 'Broker' partners and encourage interested parties to contact the SOFA team for further API information.

## DNT

### Recommended DNT RFQ list Inquiry

```
GET /rfq/dnt/recommended-list
```

**Input parameters**

| **Field Name**       | **Required**  | **Type** | **Description**                                                                                           |
| -------------------- | ------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| vault              | true          | string   | Contract information                                                                                                   |
| chainId            | true          | int      | Chain ID                                                                                                               |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | list[object] | As shown below                                               |

Object

| **Field Name**               | **Type**     | **Description**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | Chain ID                                                                                          |
| vault                        | string       | Contract address                                                                                  |
| riskType                     | string       | Risk type: PROTECTED, RISKY                                                                       |
| forCcy                       | string       | Underlying currency                                                                               |
| domCcy                       | string       | Currency pair                                                                                     |
| depositCcy                   | string       | Subscription currency                                                                             |
| lowerBarrier                 | number       | Lower price                                                                                       |
| upperBarrier                 | number       | Upper price                                                                                       |
| depositAmount                | number       | RFQ purchase amount                                                                               |
| expiry                       | number         | Expiry timestamp (e.g., 1672387200)                                                               |
| timestamp                    | number         | Current pricing's trigger time; the next observation start time is calculated based on this logic |
| observationStart             | number         | Estimated start time of observation for knocking in/out based on timestamp                        |
| feeRate             | object         | trading and settlement fee rate (optional)                        |
| leverageInfo             | object         | Loan information (optional)                        |
| relevantDollarPrices             | list[object]         | Token price required for RCH price conversion and airdrop calculation (optional)                       |
| amounts             | object         | Calculated amounts (optional)                       |
| apyInfo             | object         | Annualized information, available for non-Surge products (optional)                     |
| oddsInfo             | object         | Odds information, available for Surge products (optional)                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Maker's collateral amount                                                                         |
| \> totalCollateral            | string       | Total collateral amount (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Required when guaranteed (optional)                                                           |
| \> makerBalanceThreshold | string       | maker balance threshold                                                          |
| \> deadline                   | number         | Expiration timestamp (e.g., 1672387200)                                                           |
| \> makerWallet                | string       | Maker's wallet (optional)                                                                     |
| \> signature                  | string       | Signature (optional)                                                                          |

**Request Example**

```
GET rfq/dnt/recommended-list?vault=xxxxxx&chainId=1
```

**Response**

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

### DNT Inquiry

- Notes:
  - Please do not pass the user's wallet address on a pure inquiry request.
  - A user's wallet address should be passed on only upon subscribing.

```
GET /rfq/dnt/quote
```

**Input Parameters**

| **Field Name**     | **Required** | **Type** | **Description**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | Contract information                                                                                                   |
| chainId            | true         | int      | Chain ID                                                                                                               |
| expiry             | true         | number     | Second-level timestamp for the expiry date, e.g., 1672387200                                                           |
| lowerBarrier       | true         | number   | Lower price                                                                                                            |
| upperBarrier       | true         | number   | Upper price                                                                                                            |
| depositAmount      | true         | number   | RFQ purchase amount                                                                                                    |
| inputApyDefinition | true         | string   | The underlying code is Enum indicating how the input APY is calculated: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | Guaranteed annual yield (empty for RISKY, required for protected)                                                      |
| fundingApy         | false        | number   | AAVE annual yield (empty for RISKY, required for protected)                                                            |
| takerWallet        | false        | string   | Inquirer's wallet public address information                                                                           |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | object       | As shown below                                 |

Object

| **Field Name**               | **Type**     | **Description**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | Contract address                                                                                  |
| chainId                      | int          | Chain ID                                                                                          |
| riskType                     | string       | Risk type: PROTECTED, RISKY                                                                       |
| forCcy                       | string       | Underlying currency                                                                               |
| domCcy                       | string       | Currency pair                                                                                     |
| depositCcy                   | string       | Subscription currency                                                                             |
| lowerBarrier                 | number       | Lower price                                                                                       |
| upperBarrier                 | number       | Upper price                                                                                       |
| depositAmount                | number       | RFQ purchase amount                                                                               |
| expiry                       | number         | Expiry timestamp (e.g., 1672387200)                                                               |
| timestamp                    | number         | Current pricing's trigger time; the next observation start time is calculated based on this logic |
| observationStart             | number         | Estimated start time of observation for knocking in/out based on timestamp                        |
| feeRate             | object         | trading and settlement fee rate (optional)                        |
| leverageInfo             | object         | Loan information (optional)                        |
| relevantDollarPrices             | list[object]         | Token price required for RCH price conversion and airdrop calculation (optional)                       |
| amounts             | object         | Calculated amounts (optional)                       |
| apyInfo             | object         | Annualized information, available for non-Surge products (optional)                     |
| oddsInfo             | object         | Odds information, available for Surge products (optional)                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Maker's collateral amount                                                                         |
| \> totalCollateral            | string       | Total collateral amount (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Required when guaranteed (optional)                                                           |
| \> makerBalanceThreshold | string       | maker balance threshold                                                          |
| \> deadline                   | number         | Expiration timestamp (e.g., 1672387200)                                                           |
| \> makerWallet                | string       | Maker's wallet (optional)                                                                     |
| \> signature                  | string       | Signature (optional)                                                                          |

### DNT Winning Probability

Probability of staying within the boundaries until expiry

```
GET rfq/dnt/winning-probabilities
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | Underlying currency      |
| expiry          | true         | number     | Second-level timestamp for the expiry date, e.g., 1672387200 |
| lowerBarrier       | true         | number   | Lower price |
| upperBarrier       | true         | number   | Upper price |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | list[object] | As shown below                                 |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | number | Spot price |
| timestamp | number |  |
| probabilities | object | Winning probability  |

**Request Example**

```
GET rfq/dnt/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerBarrier=xxx&upperBarrier=xxx
```

**Response**

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

## Smart Trend

### Recommended Smart Trend RFQ list Inquiry

```
GET rfq/smart-trend/recommended-list
```

**Input parameters**

| **Field Name**       | **Required**  | **Type** | **Description**                                         |
| -------------------- | ------------- | -------- | ------------------------------------------------------------ |
| vault              | true          | string   | Contract information                               |
| chainId            | true          | int      | Chain ID                    |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | list[object] | As shown below                                 |

Object

| **Field Name**               | **Type**     | **Description**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | Chain ID                                                                                          |
| vault                        | string       | Contract address                                                                                  |
| riskType                     | string       | Risk type: PROTECTED, RISKY                                                                       |
| direction | string | BULLISH,BEARISH |   
| forCcy                       | string       | Underlying currency                                                                               |
| domCcy                       | string       | Currency pair                                                                                     |
| depositCcy                   | string       | Subscription currency                                                                             |
| lowerBarrier                 | number       | Lower price                                                                                       |
| upperBarrier                 | number       | Upper price                                                                                       |
| depositAmount                | number       | RFQ purchase amount                                                                               |
| expiry                       | number         | Expiry timestamp (e.g., 1672387200)                                                               |
| timestamp                    | number         | Current pricing's trigger time; the next observation start time is calculated based on this logic |
| feeRate             | object         | trading and settlement fee rate (optional)                        |
| leverageInfo             | object         | Loan information (optional)                        |
| relevantDollarPrices             | list[object]        | Token price required for RCH price conversion and airdrop calculation (optional)                       |
| amounts             | object         | Calculated amounts (optional)                       |
| apyInfo             | object         | Annualized information, available for non-Surge products (optional)                     |
| oddsInfo             | object         | Odds information, available for Surge products (optional)                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Maker's collateral amount                                                                         |
| \> totalCollateral            | string       | Total collateral amount (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Required when guaranteed (optional)                                                           |
| \> makerBalanceThreshold | string       | maker balance threshold                                                          |
| \> deadline                   | number         | Expiration timestamp (e.g., 1672387200)                                                           |
| \> makerWallet                | string       | Maker's wallet (optional)                                                                     |
| \> signature                  | string       | Signature (optional)                                                                          |

**Request Example**

```
GET rfq/smart-trend/recommended-list?vault=xxxxxx
```

### Smart Trend Inquiry

- Notes:
  - Please do not pass the user's wallet address on a pure inquiry request.
  - A user's wallet address should be passed on only upon subscribing.

```
GET /rfq/smart-trend/quote
```

**Input Parameters**

| **Field Name**     | **Required** | **Type** | **Description**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | Contract information                                                                                                   |
| chainId            | true         | int      | Chain ID                                                                                                               |
| expiry             | true         | number     | Second-level timestamp for the expiry date, e.g., 1672387200                                                           |
| lowerBarrier       | true         | number   | Lower price                                                                                                            |
| upperBarrier       | true         | number   | Upper price                                                                                                            |
| depositAmount      | true         | number   | RFQ purchase amount                                                                                                    |
| inputApyDefinition | true         | string   | The underlying code is Enum indicating how the input APY is calculated: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | Guaranteed annual yield (empty for RISKY, required for protected)                                                      |
| fundingApy         | false        | number   | AAVE annual yield (empty for RISKY, required for protected)                                                            |
| takerWallet        | false        | string   | Inquirer's wallet public address information                                                                           |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | object       | As shown below                                 |

Object

| **Field Name**               | **Type**     | **Description**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | Contract address                                                                                  |
| chainId                      | int          | Chain ID                                                                                          |
| riskType                     | string       | Risk type: PROTECTED, RISKY                                                                       |
| direction                    | string       | BULLISH,BEARISH | 
| forCcy                       | string       | Underlying currency                                                                               |
| domCcy                       | string       | Currency pair                                                                                     |
| depositCcy                   | string       | Subscription currency                                                                             |
| lowerBarrier                 | number       | Lower price                                                                                       |
| upperBarrier                 | number       | Upper price                                                                                       |
| depositAmount                | number       | RFQ purchase amount                                                                               |
| expiry                       | number         | Expiry timestamp (e.g., 1672387200)                                                               |
| timestamp                    | number         | Current pricing's trigger time; the next observation start time is calculated based on this logic |
| feeRate             | object         | trading and settlement fee rate (optional)                        |
| leverageInfo             | object         | Loan information (optional)                        |
| relevantDollarPrices             | list[object]         | Token price required for RCH price conversion and airdrop calculation (optional)                       |
| amounts             | object         | Calculated amounts (optional)                       |
| apyInfo             | object         | Annualized information, available for non-Surge products (optional)                     |
| oddsInfo             | object         | Odds information, available for Surge products (optional)                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Maker's collateral amount                                                                         |
| \> totalCollateral            | string       | Total collateral amount (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Required when guaranteed (optional)                                                           |
| \> makerBalanceThreshold | string       | maker balance threshold                                                          |
| \> deadline                   | number         | Expiration timestamp (e.g., 1672387200)                                                           |
| \> makerWallet                | string       | Maker's wallet (optional)                                                                     |
| \> signature                  | string       | Signature (optional)                                                                          |

### Smart Trend Winning Probability

Probability of staying within the boundaries until expiry

```
GET rfq/smart-trend/winning-probabilities
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | Underlying currency      |
| expiry          | true         | number     | Second-level timestamp for the expiry date, e.g., 1672387200 |
| lowerBarrier       | true         | number   | Lower price |
| upperBarrier       | true         | number   | Upper price |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | list[object] | As shown below                                 |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | number | Spot price |
| timestamp | number |  |
| probabilities | object | Winning probability  |


**Request Example**

```
GET rfq/smart-trend/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerStrike=xxx&upperStrike=xxx
```

**Response**

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


## Generic Interface

### Delete RFQ

Delete the RFQ that has been on-chain

```
POST rfq/remove
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| rfqId          | true         | number       | .       |

**Request Example**

```
{  
"rfqId":123456  
}
```

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | object       | .                               |


### Transaction notification

```
POST rfq/trade
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| quotes | true | list[object] |   |
| \> rfqId | true | number |     |
| \> quoteId | true | number |     |
| \> txId | true | string | Transaction hash |
| code | false | string | Invitation code |
| walletType | false | string | Wallet type, such as MetaMask, OKX Wallet, Coinbase, etc. |

**Request Example**

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

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | object       | .                               |

### Strike list

```
GET rfq/strike-list
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| indexPrice | true | number |  |
| forCcy | false | string | For the BTC-USDT trading pair, enter BTC here; if not provided, the default configuration will be used. |
| domCcy | false | string | For the BTC-USDT trading pair, enter USDT here; if not provided, it defaults to USDT. |


**Request Example**

```
{  
"indexPrice": 3750.8，  
"forCcy": "WBTC",  
}
```

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| strikes | list[number] | Default recommended strike price list |

### Expiry list

```
GET rfq/expiry-list
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| vault | true | string | Contract address |
| chainId | true | int | Chain ID |


**Request Example**

```
{  
"vault": "XXXXXXXXXXXXXX",  
"chainId": 1,  
}
```

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| timestamp | number | Baseline start time in seconds timestamp |
| expiries | list[number] | Supported expiry list in seconds timestamp, e.g., 1672387200 |

### Aave apy

```
GET rfq/aave-apy
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | Chain ID |
| ccy | string | Deposit currency |
| avgApy | string | Average APY recorded on-chain over the past 30 days |
| currentApy | string | Latest AAVE APY |
| apyUsed | string | The APY actually used in SofaServer to estimate future interest income |
| apyDefinition | string | Calculation definition corresponding to APY |

**Response Example**

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

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |
| apyDefinition | false | string | Calculation definition corresponding to APY; defaults to AaveLendingApy |


**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | Chain ID |
| ccy | string | Deposit currency |
| avgApy | string | Average APY recorded on-chain over the past 30 days |
| currentApy | string | Latest APY |
| apyUsed | string | The APY actually used in SofaServer to estimate future interest income |
| apyDefinition | string | Calculation definition corresponding to APY |

**Response Example**

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

### Get Wallet Positions

```
POST /rfq/position-list
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | Set of contract addresses; if not provided, all contracts will be queried. |
| claimed | false | boolean | Whether it has been redeemed; if not provided, positions of all statuses will be queried. |
| expired | false | boolean | Whether it has expired; if not provided, positions of all statuses will be queried. |
| concealed | false | boolean | Whether it is hidden; if not provided, positions of all statuses will be queried. |
| positiveReturn | false | boolean | Whether the redemption amount is greater than 0; if not provided, all positions will be queried. |
| positiveProfit | false | boolean | Whether the return exceeds the principal; if not provided, all positions will be queried. |
| limit | false | Int | Number of queries; defaults to 100, maximum is 300. |
| startDateTime | false | number | Corresponding timestamp in seconds (inclusive), e.g., 1672387200. |
| endDateTime | false | number | Corresponding timestamp in seconds (inclusive), e.g., 1672387200. |
| orderBy | false | string | "createdAt" or "return", sorting method: "createdAt" (update time, default) or "return" (returns). |
| orderDirection | false | string | "desc" or "asc", defaults to "desc" (descending order). |
| wallet | false | string | Wallet address (if empty, all wallet addresses are queried). |

Note: If the number of results exceeds 300 within the specified `startDateTime` and `endDateTime`, polling can be used (in this case, `orderBy` must be set to `"createdAt"`) to continue querying. The `endDateTime` parameter for the nth polling request should be set to the `createdAt` of the last record from the (n-1)th polling result. After retrieving all data, duplicates should be removed based on the `id` field.

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | As shown below |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string | Product ID corresponding to the position (matches `productId` in The Graph data). This ID is unique within the chain + vault dimension and can be used to query position balances. |
| positionId | string | Position ID, which is unique within the chain dimension. |
| product | object | Product information. |
| wallet | string | Wallet address. |
| createdAt | number | Corresponding timestamp in seconds, e.g., 1672387200. |
| updatedAt | number | Corresponding timestamp in seconds, e.g., 1672387200. |
| claimed | boolean | Whether the position has been redeemed. |
| takerAllocationRate | number | Based on the calculation time, estimates the proportion the owner can take from the betting pool. Before expiry, this is an estimated value. |
| triggerTime | number | For Rangebound, the first breakout time; for non-Rangebound, the settlement time, in seconds. |
| triggerPrice | number | For Rangebound, the first breakout price; for non-Rangebound, the settlement price. |
| feeRate | object | Fee rate. |
| leverageInfo | object | Loan information (optional). |
| relevantDollarPrices | list[object] | Token prices required for RCH price conversion and airdrop calculation. |
| amounts | object | Calculated amounts. |
| apyInfo | object | Annualized information, available for non-Surge products. |
| oddsInfo | object | Odds information, available for Surge products. |
| claimParams | object | Claim parameter information. |

**Response Example**

```
{
    "value": [{
        ...
    }]
}
```

### Get Wallet Transactions

```
POST /rfq/transaction-list
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | Set of contract addresses; if not provided, all contracts will be queried. |
| limit | false | Int | Number of queries; defaults to 100, maximum is 300. |
| startDateTime | false | number | Corresponding timestamp in seconds (inclusive), e.g., 1672387200. |
| endDateTime | false | number | Corresponding timestamp in seconds (inclusive), e.g., 1672387200. |
| orderDirection | false | string | "desc" or "asc", defaults to "desc" (descending order). |
| taker | false | string | Taker wallet address |
| maker | false | string | Maker wallet address |
| claimParams | false | object | The redemption parameters, corresponding to the claimParams field in the position-list data. |
| hash | false | string | Transaction hash |

Note: Taker, maker, and claimParams parameters can be used for a joint query to find the trade records corresponding to a position (a position may correspond to multiple trades) (not all parameters are required).
If the number of results exceeds 300 within the specified `startDateTime` and `endDateTime`, polling can be used (in this case, `orderBy` must be set to `"createdAt"`) to continue querying. The `endDateTime` parameter for the nth polling request should be set to the `createdAt` of the last record from the (n-1)th polling result. After retrieving all data, duplicates should be removed based on the `id` field.

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | As shown below |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | Transaction hash |
| takerWallet | string | taker wallet address. |
| makerWallet | string | maker wallet address. |
| product | object | Product information. |
| createdAt | number | Corresponding timestamp in seconds, e.g., 1672387200. |
| takerAllocationRate | number | Based on the calculation time, estimates the proportion the owner can take from the betting pool. Before expiry, this is an estimated value. |
| triggerTime | number | For Rangebound, the first breakout time; for non-Rangebound, the settlement time, in seconds. |
| triggerPrice | number | For Rangebound, the first breakout price; for non-Rangebound, the settlement price. |
| feeRate | object | Fee rate. |
| leverageInfo | object | Loan information (optional). |
| relevantDollarPrices | list[object] | Token prices required for RCH price conversion and airdrop calculation. |
| amounts | object | Calculated amounts. |
| apyInfo | object | Annualized information, available for non-Surge products. |
| oddsInfo | object | Odds information, available for Surge products. |

**Response Example**

```
{
    "value": [{
        ...
    }]
}
```

### Conceal loss positions

```
POST rfq/position/conceal
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | true | int | Chain ID |
| positionIds | true | List[string] | positionId list，up to 20 |

**Request Example**

```
{
    "positionIds": [
        "aaaa","bbbb"
    ],
    "chainId": 1
}
```

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | object       | .                               |


### Get Pending Transactions

This is mainly to address the underlying position data synchronization issue. The data retrieved here may disappear (due to blockchain fork competition).

```
POST rfq/transactions/pending
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | Set of contract addresses; if not provided, all contracts will be queried. |
| taker | false | string | Taker wallet address |
| maker | false | string | Maker wallet address |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | As shown below |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | Transaction hash |
| takerWallet | string | taker wallet address. |
| makerWallet | string | maker wallet address. |
| product | object | Product information. |
| createdAt | number | Corresponding timestamp in seconds, e.g., 1672387200. |
| takerAllocationRate | number | Based on the calculation time, estimates the proportion the owner can take from the betting pool. Before expiry, this is an estimated value. |
| triggerTime | number | For Rangebound, the first breakout time; for non-Rangebound, the settlement time, in seconds. |
| triggerPrice | number | For Rangebound, the first breakout price; for non-Rangebound, the settlement price. |
| feeRate | object | Fee rate. |
| leverageInfo | object | Loan information (optional). |
| relevantDollarPrices | list[object] | Token prices required for RCH price conversion and airdrop calculation. |
| amounts | object | Calculated amounts. |
| apyInfo | object | Annualized information, available for non-Surge products. |
| oddsInfo | object | Odds information, available for Surge products. |


### Get RCH Airdrop History

```
GET rfq/airdrop/history
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| wallet | true | string | Wallet address|
| startDateTime | true | number | Corresponding timestamp in seconds (inclusive), e.g., 1672387200. |
| endDateTime | true | number | Corresponding timestamp in seconds (inclusive), e.g., 1672387200. |
| orderBy | false | string | "dateTime" or "rch", sorting method: "dateTime" (airdrop time, default) or "rch" (amount). |
| orderDirection | false | string | "desc" or "asc", defaults to "desc" (descending order). |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | As shown below |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| dateTime | long | The current timestamp in seconds, for example, 1672387200. |
| wallet | string | Wallet address |
| volume | string | trading volume |
| rch | string | Airdrop amount, the raw value, needs to be divided by 1e18 |
| merkleProof | string | Merkle proof  |

