# dApp Open APIs (ie. Broker)

Our existing dApp on sofa.org is a critical workflow feature and is the main way for users to interact with the SOFA protocols.  However, we encourage other developers to enable access and connect to SOFA via their own dApps as well to maximize our ecosystem growth.  We consider these to be our 'Broker' partners and encourage interested parties to contact the SOFA team for further API information.

## Recommended Range (DNT) RFQ Inquiry

- Description
  - Supports a list of RFQs composed of the same vault+chainId.

```
POST /rfq/dnt/recommended-quote
```

**Input parameters**

| **Field Name**       | **Required**  | **Type** | **Description**                                                                                                        |
| -------------------- | ------------- | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| list                 | array[object] |          |                                                                                                                        |
| vault              | true          | string   | Contract information                                                                                                   |
| chainId            | true          | int      | Chain ID                                                                                                               |
| expiry             | true          | long     | Second-level timestamp for the expiry date, e.g., 1672387200                                                           |
| lowerBarrier       | true          | number   | Lower price                                                                                                            |
| upperBarrier       | true          | number   | Upper price                                                                                                            |
| depositAmount      | true          | number   | RFQ purchase amount                                                                                                    |
| inputApyDefinition | true          | string   | The underlying code is Enum indicating how the input APY is calculatedOptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false         | number   | Guaranteed annual yield (empty for RISKY, required for protected)                                                      |
| fundingApy         | false         | number   | AAVE annual yield (empty for RISKY, required for protected)                                                            |

**Input Parameter Example**

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

**Output Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | list[object] |                                                |

Object

| **Field Name**               | **Type**     | **Description**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| vault                        | string       | Contract address                                                                                  |
| chainId                      | int          | Chain ID                                                                                          |
| riskType                     | string       | Risk type: PROTECTED, RISKY                                                                       |
| forCcy                       | string       | Underlying currency                                                                               |
| domCcy                       | string       | Currency pair                                                                                     |
| depositCcy                   | string       | Subscription currency                                                                             |
| lowerBarrier                 | number       | Lower price                                                                                       |
| upperBarrier                 | number       | Upper price                                                                                       |
| depositAmount                | number       | RFQ purchase amount                                                                               |
| expiry                       | long         | Expiry timestamp (e.g., 1672387200)                                                               |
| protectedReturnAmount        | number       | Total amount of guaranteed return, in the same unit as depositCcy                                 |
| fundingAmount                | number       | Estimated yield for the period; zero for non-guaranteed types                                     |
| enhancedReturnAmount         | number       | Total amount of extra return when not knocked out, in the same unit as depositCcy                 |
| timestamp                    | long         | Current pricing's trigger time; the next observation start time is calculated based on this logic |
| observationStart             | long         | Estimated start time of observation for knocking in/out based on timestamp                        |
| quote                        | object       |                                                                                                   |
| anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| makerCollateral            | string       | Maker's collateral amount                                                                         |
| totalCollateral            | string       | Total collateral amount (Taker+Maker)                                                             |
| collateralAtRiskPercentage | string       | Required when guaranteed (can be empty)                                                           |
| deadline                   | long         | Expiration timestamp (e.g., 1672387200)                                                           |
| makerWallet                | string       | Maker's wallet (can be empty)                                                                     |
| signature                  | string       | Signature (can be empty)                                                                          |

Output Parameters example

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

## Inquiry

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
| expiry             | true         | long     | Second-level timestamp for the expiry date, e.g., 1672387200                                                           |
| lowerBarrier       | true         | number   | Lower price                                                                                                            |
| upperBarrier       | true         | number   | Upper price                                                                                                            |
| depositAmount      | true         | number   | RFQ purchase amount                                                                                                    |
| inputApyDefinition | true         | string   | The underlying code is Enum indicating how the input APY is calculatedOptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | Guaranteed annual yield (empty for RISKY, required for protected)                                                      |
| fundingApy         | false        | number   | AAVE annual yield (empty for RISKY, required for protected)                                                            |
| takerWallet        | false        | string   | Inquirer's wallet public address information                                                                           |

**Output Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicates that the return result is normal   |
| message        | string       | Error message returned in case of an exception |
| value          | list[object] |                                                |

Object

| **Field Name**               | **Type**     | **Description**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| vault                        | string       | Contract address                                                                                  |
| chainId                      | int          | Chain ID                                                                                          |
| riskType                     | string       | Risk type: PROTECTED, RISKY                                                                       |
| forCcy                       | string       | Underlying currency                                                                               |
| domCcy                       | string       | Currency pair                                                                                     |
| depositCcy                   | string       | Subscription currency                                                                             |
| lowerBarrier                 | number       | Lower price                                                                                       |
| upperBarrier                 | number       | Upper price                                                                                       |
| depositAmount                | number       | RFQ purchase amount                                                                               |
| expiry                       | long         | Expiry timestamp (e.g., 1672387200)                                                               |
| protectedReturnAmount        | number       | Total amount of guaranteed return, in the same unit as depositCcy                                 |
| fundingAmount                | number       | Estimated yield for the period; zero for non-guaranteed types                                     |
| enhancedReturnAmount         | number       | Total amount of extra return when not knocked out, in the same unit as depositCcy                 |
| timestamp                    | long         | Current pricing's trigger time; the next observation start time is calculated based on this logic |
| observationStart             | long         | Estimated start time of observation for knocking in/out based on timestamp                        |
| quote                        | object       |                                                                                                   |
| anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| makerCollateral            | string       | Maker's collateral amount                                                                         |
| totalCollateral            | string       | Total collateral amount (Taker+Maker)                                                             |
| collateralAtRiskPercentage | string       | Required when guaranteed (can be empty)                                                           |
| deadline                   | long         | Expiration timestamp (e.g., 1672387200)                                                           |
| makerWallet                | string       | Maker's wallet (can be empty)                                                                     |
| signature                  | string       | Signature (can be empty)                                                                          |
