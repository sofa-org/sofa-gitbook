# 마켓 메이커를 위한 오픈 API

## 사양

### 접두사

1. 거래 보안을 보장하기 위해 전송에 HTTPS를 사용합니다.
2. JSON은 데이터 교환 형식입니다.
3. UTF-8 문자 인코딩이 보편적으로 적용됩니다.
4. 인터페이스 서명 알고리즘은 HMAC-SHA256을 사용합니다.
5. UNIX 밀리초 타임스탬프가 사용되며, 1970년 1월 1일 0:00:00 이후의 밀리초 수를 나타냅니다.

### 매개변수

#### 요청

| 이름          | 유형   | 비고                                                  |
| ------------- | ------ | ----------------------------------------------------- |
| H-Request-Id  | 문자열 | [헤더]요청 ID, 고유                                   |
| H-Api-Key     | 문자열 | [헤더]API 키                                         |
| H-Timestamp   | 긴     | [헤더]유효한 타임스탬프, 예: 1672387200000           |
| H-Nonce       | 문자열 | [헤더]무작위 문자열                                   |
| Authorization | 문자열 | [헤더]서명, 예: [mm_id]-hmac-sha256 서명            |

#### 응답

| 이름    | 유형    | 비고               |
| ------- | ------- | ------------------ |
| 코드    | 정수    | [본문]오류 코드    |
| 메시지  | 문자열  | [본문]오류 이유    |
| 값     | T       | [본문]결과         |

### 서명 생성

우리의 RFQ 플랫폼은 요청을 승인하기 위해 파트너 서명이 필요하며, 그 뒤에 검증 절차가 이어집니다. 검증 실패 시 플랫폼은 `401` Unauthorized 응답으로 거부됩니다.

#### 서명 문자열 구성:

서명 문자열은 다섯 개의 줄로 구성되며, 각 줄은 하나의 매개변수를 나타냅니다. 각 줄은 세미콜론으로 끝나며, 마지막 줄도 포함됩니다. 유효한 타임스탬프와 요청 논스는 각각 헤더의 `H-Timestamp` 및 `H-Nonce` 매개변수에서 가져옵니다.

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### 서명 채우기

`SecretKey`를 사용하여 `StringToSign`을 `HMAC-SHA256`으로 암호화합니다.

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### HTTP 헤더 설정

요청은 `HTTP Authorization` 헤더를 통해 서명을 전송합니다. `Authorization header`는 두 부분으로 구성됩니다: **인증 유형**과 **서명 정보**.

```
Authorization: AuthenticationType SignatureInformation
```

1. 인증 유형: `[mm_id]-hmac-sha256`
2. 서명 정보: `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_참고:_

- 요청 방법은 대문자로 작성해야 합니다.
- 서명에 사용되는 `RequestBody`는 요청 본문의 내용과 일치해야 합니다.
- GET 및 DELETE 요청의 경우, `URI`는 요청 매개변수를 포함해야 합니다 (예: /api/v1/result?orderId=123).
  - 요청 본문이 없는 경우(일반적으로 GET 요청의 경우), 요청 본문은 빈 문자열("")이어야 합니다.
- 유효한 타임스탬프(H-Timestamp)는 요청자가 결정하며, 유효한 타임스탬프를 초과하는 요청은 RFQ 서버에 의해 거부됩니다.

## RFQ

### DNT RFQ 견적 제공

```
GET rfq/dnt/quote
```

**매개변수**

| 이름                     | 필수    | 유형    | 설명                                                             |
| ----------------------- | ------- | ------ | ----------------------------------------------------------------- |
| vault                   | true    | string | 금고 주소                                                         |
| chainId                 | true    | int    | 체인 ID                                                          |
| expiry                  | true    | long   | 만료 시간(초 단위 타임스탬프), 예: 1672387200                   |
| lowerBarrier            | true    | number | 하한 장벽                                                         |
| upperBarrier            | true    | number | 상한 장벽                                                         |
| depositAmount           | true    | number | 예치금                                                           |
| premiumAmount           | true    | number | 프리미엄                                                           |
| protectedFundingAmount  | false   | number | 보호된 이자(위험이 있을 경우 null)                               |
| deadline                | true    | long   | 견적 마감 시간, 예: 1672387200                                   |
| takerWallet             | false   | string | 수취인 지갑 주소                                                 |
| anchorPricesDecimal     | true    | long   |                                                                   |
| makerCollateralDecimal  | true    | long   |                                                                   |
| collateralAtRiskDecimal | true    | long   |                                                                   |
| totalCollateralDecimal  | true    | long   |                                                                   |
| underlyingPair          | true    | string | 기초 쌍, 예: BTC-USDT                                            |
| trackingSource          | true    | string | 기초 가치를 추적하는 데 사용되는 데이터 소스, 예: DERIBIT      |
| depositCoin             | true    | string | DNT 구독을 위해 지불된 프리미엄의 통화/코인                     |
| tradingFeeRate          | true    | number |                                                                   |
| settlementFeeRate       | true    | number |                                                                   |
| riskType                | true    | string | 유형: PROTECTED, RISKY                                           |

**응답**

| 이름                       | 유형         | 설명                             |
| -------------------------- | ------------ | --------------------------------- |
| timestamp                  | long         | 견적 타임스탬프                  |
| vault                      | string       |                                   |
| chainId                    | int          |                                   |
| expiry                     | long         | 만료 타임스탬프, 예: 1672387200  |
| anchorPrices               | list[string] | 20000000000,30000000000           |
| makerCollateral            | string       |                                   |
| totalCollateral            | string       |                                   |

| collateralAtRisk           | string       | E18                                |
| makerBalanceThreshold      | string       |                                    |
| deadline                   | long         | 견적 마감 타임스탬프               |
| makerWallet                | string       |                                    |
| signature                  | string       | .                                   |

Note:

1. $$collateralAtRisk - makerCollateral ==  premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**예시**

요청 URL

```
rfq/dnt/quote
```

매개변수

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
### 상승/하락 추세 RFQ 견적 제공

```

GET rfq/smart-trend/quote
```

**매개변수**

| 이름                    | 필수     | 유형    | 설명                                                             |
| ----------------------- | -------- | ------- | ----------------------------------------------------------------- |
| vault                   | true     | string  | 금고 주소                                                        |
| chainId                 | true     | int     | 체인 ID                                                          |
| expiry                  | true     | long    | 만료 시간(초 단위 타임스탬프), 예: 1672387200                   |
| direction               | true     | string  | 상승 / 하락                                                       |
| lowerStrike             | true     | number  | 하한 장벽                                                       |
| upperStrike             | true     | number  | 상한 장벽                                                       |
| depositAmount           | true     | number  | 예치금                                                           |
| premiumAmount           | true     | number  | 프리미엄                                                         |
| protectedFundingAmount  | false    | number  | 보호된 이자(위험이 있을 경우 null)                               |
| deadline                | true     | long    | 견적 마감 시간, 예: 1672387200                                   |
| takerWallet             | false    | string  | 테이커 지갑 주소                                                |
| anchorPricesDecimal     | true     | long    |                                                                 |
| makerCollateralDecimal  | true     | long    |                                                                 |
| collateralAtRiskDecimal | true     | long    |                                                                 |
| totalCollateralDecimal  | true     | long    |                                                                 |
| underlyingPair          | true     | string  | 기초 쌍, 예: BTC-USDT                                           |
| trackingSource          | true     | string  | 기초 가치를 추적하는 데 사용되는 데이터 소스, 예: DERIBIT     |
| tradingFeeRate          | true     | number  |                                                                 |
| settlementFeeRate       | true     | number  |                                                                 |
| depositCoin             | true     | string  | 스마트 트렌드 구독을 위해 지불된 프리미엄의 통화 / 코인        |
| riskType                | true     | string  | 유형: 보호됨, 위험 있음                                         |

**응답**

| 이름              | 유형         | 설명                               |
| ----------------- | ------------ | ---------------------------------- |
| timestamp         | long         | 견적 타임스탬프                    |
| vault             | string       |                                    |
| chainId           | int          |                                    |
| expiry            | long         | 만료 타임스탬프, 예: 1672387200   |
| anchorPrices      | list[string] | 20000000000,30000000000            |
| makerCollateral   | string       |                                    |
| totalCollateral   | string       |                                    |
| collateralAtRisk | string       | E18                                |
| deadline         | long         | 견적 마감 타임스탬프                  |
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

### 이중 RFQ 견적 제공

```
GET rfq/dual/quote
```

**매개변수**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | 금고 주소                                                        |
| chainId                 | true     | int    | 체인 ID                                                          |
| expiry                  | true     | long   | 만료 시간(초 단위 타임스탬프), 예: 1672387200                   |
| strike                  | true     | number | 행사가격                                                         |
| type                    | true     | string | CALL 또는 PUT                                                    |
| depositAmount           | true     | number | 예치금                                                          |
| deadline                | true     | long   | 견적 마감, 예: 1672387200                                        |
| refDateTime             | true     | long   | 현재 요청 시간                                                  |
| takerWallet             | false    | string | 테이커 지갑 주소                                                |
| anchorPriceDecimal      | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | 기초 자산 쌍, 예: BTC-USDT                                      |
| trackingSource          | true     | string | 기초 가치를 추적하는 데 사용되는 데이터 소스, 예: DERIBIT    |
| depositCoin             | true     | string | 이중 구독을 위해 지불된 프리미엄의 통화 / 코인               |
| depositCoinTokenAddress | true     | string |                                                                   |
| depositCoinTokenDecimal | true     | long   |                                                                   |
| tradingFeeRate          | true     | number | .                                                                  |

**Response**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | 견적 타임스탬프                    |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | 만료 타임스탬프, 예: 1672387200   |
| anchorPrice      | string       | 20000000000                        |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| deadline         | long         | 견적 마감 타임스탬프              |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

## Appendix

| Code | Message                                                          |
| ---- | ---------------------------------------------------------------- |
| 1000 | 시스템 오류입니다.                                               |
| 2001 | 서명 오류입니다.                                                |
| 2002 | 매개변수 오류입니다.                                           |
| 3001 | 요청한 정보가 존재하지 않습니다.                              |
| 3002 | 입금 금액이 _depositRange_를 벗어났습니다.                     |
| 3003 | 최대 구독 한도에 도달했습니다.                                 |
| 3004 | 프리미엄 금액의 차이가 너무 커서 구독에 실패했습니다.          |
| 3005 | 견적에 실패했습니다.                                           |
| 3006 | 일시적으로 서비스를 제공하지 않습니다.                        |
| 3007 | API 속도 제한을 초과했습니다. 속도를 줄여보세요.              |
| 3100 | 주문 생성에 실패했습니다.                                      |

**Required Functions**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$
