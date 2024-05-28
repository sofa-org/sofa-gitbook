# Open APIs for Market Maker

## Specification

### Prefix

1. To ensure transaction security, use HTTPS for transmission.
2. JSON is the data interchange format.
3. UTF-8 character encoding is universally applied.
4. Interface signing algorithm utilizes HMAC-SHA256.
5. UNIX millisecond timestamp is used, representing the number of milliseconds since January 1, 1970, 0:00:00.

### Parameters

1. Request

| name          | type   | remark                                              |
| ------------- | ------ | --------------------------------------------------- |
| H-Request-Id  | string | [Header]Request ID, unique                          |
| H-Api-Key     | string | [Header]API Key                                     |
| H-Timestamp   | long   | [Header]Valid timestamp，e.g., 1672387200000        |
| H-Nonce       | string | [Header]Random string                               |
| Authorization | string | [Header]Signature，e.g.,：rfq-hmac-sha256 signature |

2. Rsponse

| name    | type    | ramark             |
| ------- | ------- | ------------------ |
| code    | integer | [Body]Error code   |
| message | string  | [Body]Error reason |
| value   | T       | [Body]Result       |

### Signature Generation

Our RFQ platform requires partner signatures to approve requests, followed by a verification procedure.  A verification failure will lead to a platform rejection with a `401` Unauthorized response.

1. Building the signature string:

The signature string consists of five lines, each representing a parameter.  Each line ends with a semicolon, including the last line.  The valid timestamp and the request nonce are taken from the `H-Timestamp` and `H-Nonce` parameters in the header, respectively.

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

2. Populating the signature

Use the `SecretKey` to encrypt `StringToSign` using HMAC-SHA256.

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

3. Setting HTTP Headers

The request transmits the signature through the `HTTP Authorization` header. The `Authorization header` comprises of two parts: **authentication type** and **signature information**.

```
Authorization: AuthenticationType SignatureInformation
```

    1. Authentication Type: rfq `-hmac-sha256`
    2. Signature Information: `Signature`

```
Authorization: rfq-hmac-sha256 Signature
```

_Note:_

- The request method should be in UPPERCASE.
- The `RequestBody` used for the signature must match the content of the request's body.
- For GET and DELETE requests, the `URI` should include the request parameters (e.g., /api/v1/result?orderId=123).

  - If there is no request body (common for GET requests), the request body should be an empty string ("").
- The valid timestamp (H-Timestamp) is determined by the requester; requests extending beyond the valid timestamp will be rejected by the RFQ server.

## RFQ

### Provide DNT RFQ Quote

```
GET rfq/dnt/quote
```

**Parameters**

| name                              | required | type   | description                                                       |
| --------------------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                             | true     | string | Vault address                                                     |
| chainId                           | true     | int    | Chain ID                                                          |
| expiry                            | true     | long   | Expiry time in seconds timestamp，e.g., 1672387200                |
| lowerBarrier                      | true     | number | Lower barrier                                                     |
| upperBarrier                      | true     | number | Upper barrier                                                     |
| depositAmount                     | true     | number | Deposit                                                           |
| premiumAmount                     | true     | number | Premium                                                           |
| protectedFundingAmount            | false    | number | Protected interest（null when RISKY）                             |
| deadline                          | true     | long   | Quote deadline，e.g., 1672387200                                  |
| takerWallet                       | false    | string | Taker wallet address                                              |
| anchorPricesDecimal               | true     | long   |                                                                   |
| makerCollateralDecimal            | true     | long   |                                                                   |
| collateralAtRiskPercentageDecimal | true     | long   |                                                                   |
| totalCollateralDecimal            | true     | long   |                                                                   |
| underlyingPair                    | true     | string | Underlying Pair, e.g. BTC-USDT                                    |
| trackingSource                    | true     | string | Data sources are used to track the underlying value, e.g. DERIBIT |
| depositCoin                       | true     | string | Currency / Coin of the premium paid to subscribe DNT              |
| riskType                          | true     | string | Type：PROTECTED, RISKY                                            |

**Response**

| name                       | type         | description                        |
| -------------------------- | ------------ | ---------------------------------- |
| timestamp                  | long         | Quote timestamp                    |
| vault                      | string       |                                    |
| chainId                    | int          |                                    |
| expiry                     | long         | Expiry timestamp，e.g., 1672387200 |
| anchorPrices               | list[string] | 20000000000,30000000000            |
| makerCollateral            | string       |                                    |
| totalCollateral            | string       |                                    |
| collateralAtRiskPercentage | string       | E18                                |
| deadline                   | long         | Quote deadline timestamp           |
| makerWallet                | string       |                                    |
| signature                  | string       |                                    |

Note:

1. totalCollateral * collateralAtRiskPercentage - makerCollateral ==  premiumAmount
2. totalCollateral  - makerCollateral == depositAmount

**Examples**

Request URL

```
rfq/dnt/quote
```

Parameters

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

## Appendix

| Code | Message                                                          |
| ---- | ---------------------------------------------------------------- |
| 1000 | system error.                                                    |
| 2001 | sign error.                                                      |
| 2002 | param error.                                                     |
| 3001 | Requested information does not exist.                            |
| 3002 | Deposit amount is outside _depositRange_.                        |
| 3003 | Maximum subscriptions limit is reached.                          |
| 3004 | Subscription failed due to too much difference in premiumAmount. |
| 3005 | Quote failed.                                                    |
| 3006 | Temporarily do not provide service.                              |
| 3007 | Api rate limit exceeded. Try slow down.                          |
| 3100 | Order creation failed.                                           |

**Required Functions**：

- premiumAmount = totalCollateral * dntCollateralRatio - makerCollateral
- depositAmount = totalCollater - makerCollateral
- bookingQuantity = totalCollateral * dntCollateralRatio
- projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365
- Tenor = (expDateTime - refDateTime) / 365
- APY-In-Range = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor
- APY-Protected = (projectedFundingAmount - premiumAmount) / tenor
