# DApp Open APIs

우리의 기존 dApp인 sofa.org는 중요한 워크플로우 기능이며 사용자가 SOFA 프로토콜과 상호작용하는 주요 방법입니다. 그러나 우리는 다른 개발자들이 자신의 dApp을 통해 SOFA에 접근하고 연결하여 우리의 생태계 성장을 극대화할 것을 권장합니다. 우리는 이들을 '브로커' 파트너로 간주하며, 관심 있는 당사자들은 추가 API 정보에 대해 SOFA 팀에 연락할 것을 권장합니다.

## DNT

### 추천 DNT RFQ 목록 조회

```
GET /rfq/dnt/recommended-list
```

**입력 매개변수**

| **필드 이름**       | **필수**  | **유형** | **설명**                                                                                           |
| -------------------- | ------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| vault              | true          | string   | 계약 정보                                                                                                   |
| chainId            | true          | int      | 체인 ID                                                                                                               |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0은 반환 결과가 정상임을 나타냅니다   |
| message        | string       | 예외 발생 시 반환되는 오류 메시지 |
| value          | list[object] | 아래와 같이 표시됩니다                                               |

객체

| **필드 이름**               | **유형**     | **설명**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | 체인 ID                                                                                          |
| vault                        | string       | 계약 주소                                                                                  |
| riskType                     | string       | 위험 유형: PROTECTED, RISKY                                                                       |
| forCcy                       | string       | 기초 통화                                                                               |
| domCcy                       | string       | 통화 쌍                                                                                     |
| depositCcy                   | string       | 구독 통화                                                                             |
| lowerBarrier                 | number       | 하한 가격                                                                                       |
| upperBarrier                 | number       | 상한 가격                                                                                       |
| depositAmount                | number       | RFQ 구매 금액                                                                               |
| expiry                       | number         | 만료 타임스탬프 (예: 1672387200)                                                               |
| timestamp                    | number         | 현재 가격의 트리거 시간; 다음 관찰 시작 시간은 이 논리를 기반으로 계산됩니다 |
| observationStart             | number         | 타임스탬프를 기반으로 한 관찰 시작 시간의 추정치                        |
| feeRate                      | object         | 거래 및 정산 수수료율 (선택 사항)                        |
| leverageInfo                 | object         | 대출 정보 (선택 사항)                        |
| relevantDollarPrices         | list[object]   | RCH 가격 변환 및 에어드롭 계산에 필요한 토큰 가격 (선택 사항)                       |
| amounts                      | object         | 계산된 금액 (선택 사항)                       |
| apyInfo                     | object         | 연간화된 정보, 비서지 제품에 대해 제공 (선택 사항)                     |
| oddsInfo                     | object         | 확률 정보, 서지 제품에 대해 제공 (선택 사항)                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | 메이커의 담보 금액                                                                         |
| \> totalCollateral            | string       | 총 담보 금액 (테이커+메이커)                                                             |
| \> collateralAtRisk          | string       | 보장이 필요할 때 (선택 사항)                                                           |
| \> makerBalanceThreshold      | string       | 메이커 잔액 한도                                                          |
| \> deadline                   | number         | 만료 타임스탬프 (예: 1672387200)                                                           |
| \> makerWallet                | string       | 메이커의 지갑 (선택 사항)                                                                     |
| \> signature                  | string       | 서명 (선택 사항)                                                                          |

**요청 예시**

```
GET rfq/dnt/recommended-list?vault=xxxxxx&chainId=1
```

**응답**

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

### DNT 문의

- 참고:
  - 순수 문의 요청 시 사용자의 지갑 주소를 전달하지 마십시오.
  - 사용자의 지갑 주소는 구독 시에만 전달되어야 합니다.

```
GET /rfq/dnt/quote
```

**입력 매개변수**

| **필드 이름**     | **필수** | **유형** | **설명**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | 계약 정보                                                                                                   |
| chainId            | true         | int      | 체인 ID                                                                                                               |
| expiry             | true         | number   | 만료 날짜에 대한 초 단위 타임스탬프, 예: 1672387200                                                           |
| lowerBarrier       | true         | number   | 하한 가격                                                                                                            |
| upperBarrier       | true         | number   | 상한 가격                                                                                                            |
| depositAmount      | true         | number   | RFQ 구매 금액                                                                                                    |
| inputApyDefinition | true         | string   | 입력 APY가 계산되는 방식을 나타내는 Enum: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | 보장된 연간 수익률 (RISKY의 경우 비어 있음, protected의 경우 필수)                                                      |
| fundingApy         | false        | number   | AAVE 연간 수익률 (RISKY의 경우 비어 있음, protected의 경우 필수)                                                            |
| takerWallet        | false        | string   | 문의자의 지갑 공개 주소 정보                                                                           |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0은 반환 결과가 정상임을 나타냅니다.   |
| message        | string       | 예외 발생 시 반환되는 오류 메시지 |
| value          | object       | 아래와 같이                                 |

객체

| **필드 이름**               | **유형**     | **설명**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | 계약 주소                                                                                  |
| chainId                      | int          | 체인 ID                                                                                          |
| riskType                     | string       | 위험 유형: PROTECTED, RISKY                                                                       |
| forCcy                       | string       | 기초 통화                                                                               |
| domCcy                       | string       | 통화 쌍                                                                                     |
| depositCcy                   | string       | 구독 통화                                                                             |
| lowerBarrier                 | number       | 하한 가격                                                                                       |
| upperBarrier                 | number       | 상한 가격                                                                                       |
| depositAmount                | number       | RFQ 구매 금액                                                                               |
| expiry                       | number         | 만료 타임스탬프 (예: 1672387200)                                                               |
| timestamp                    | number         | 현재 가격의 트리거 시간; 다음 관찰 시작 시간은 이 논리를 기반으로 계산됩니다. |
| observationStart             | number         | 타임스탬프를 기반으로 한 인/아웃 관찰의 예상 시작 시간                        |
| feeRate                      | object         | 거래 및 정산 수수료율 (선택 사항)                        |
| leverageInfo                 | object         | 대출 정보 (선택 사항)                        |
| relevantDollarPrices         | list[object]         | RCH 가격 변환 및 에어드랍 계산에 필요한 토큰 가격 (선택 사항)                       |
| amounts                      | object         | 계산된 금액 (선택 사항)                       |
| apyInfo             | object         | 비연속 제품에 대해 사용할 수 있는 연간화 정보 (선택 사항)                     |
| oddsInfo             | object         | 서지 제품에 대해 사용할 수 있는 확률 정보 (선택 사항)                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | 메이커의 담보 금액                                                                         |
| \> totalCollateral            | string       | 총 담보 금액 (테이커 + 메이커)                                                             |
| \> collateralAtRisk | string       | 보장이 필요할 때 요구됨 (선택 사항)                                                           |
| \> makerBalanceThreshold | string       | 메이커 잔액 한도                                                          |
| \> deadline                   | number         | 만료 타임스탬프 (예: 1672387200)                                                           |
| \> makerWallet                | string       | 메이커의 지갑 (선택 사항)                                                                     |
| \> signature                  | string       | 서명 (선택 사항)                                                                          |

### DNT 승리 확률

만료까지 경계 내에 머무를 확률

```
GET rfq/dnt/winning-probabilities
```

**입력 매개변수**

| **필드 이름** | **필요 여부** | **유형** | **설명** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | 기초 통화      |
| expiry          | true         | number     | 만료 날짜에 대한 2차 타임스탬프, 예: 1672387200 |
| lowerBarrier       | true         | number   | 하한 가격 |
| upperBarrier       | true         | number   | 상한 가격 |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0은 반환 결과가 정상임을 나타냄   |
| message        | string       | 예외 발생 시 반환되는 오류 메시지 |
| value          | list[object] | 아래와 같이                                 |

Object

| **필드 이름** | **유형**     | **설명**                                   |
| -------------- | ------------ | ------------------------------------------ |
| spotPrice | number | 스팟 가격 |
| timestamp | number |  |
| probabilities | object | 승리 확률  |

**요청 예시**

```
GET rfq/dnt/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerBarrier=xxx&upperBarrier=xxx
```

**응답**

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

## 스마트 트렌드

### 추천 스마트 트렌드 RFQ 목록 조회

```
GET rfq/smart-trend/recommended-list
```

**입력 매개변수**

| **필드 이름**       | **필요 여부**  | **유형** | **설명**                                         |
| -------------------- | ------------- | -------- | ------------------------------------------------------------ |
| vault              | true          | string   | 계약 정보                               |
| chainId            | true          | int      | 체인 ID                    |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0은 반환 결과가 정상임을 나타냅니다.   |
| message        | string       | 예외 발생 시 반환되는 오류 메시지 |
| value          | list[object] | 아래와 같이                                 |

객체

| **필드 이름**               | **유형**     | **설명**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | 체인 ID                                                                                          |
| vault                        | string       | 계약 주소                                                                                  |
| riskType                     | string       | 위험 유형: PROTECTED, RISKY                                                                       |
| direction                    | string       | BULLISH, BEARISH |   
| forCcy                       | string       | 기초 통화                                                                               |
| domCcy                       | string       | 통화 쌍                                                                                     |
| depositCcy                   | string       | 가입 통화                                                                             |
| lowerBarrier                 | number       | 하한 가격                                                                                       |
| upperBarrier                 | number       | 상한 가격                                                                                       |
| depositAmount                | number       | RFQ 구매 금액                                                                               |
| expiry                       | number         | 만료 타임스탬프 (예: 1672387200)                                                               |
| timestamp                    | number         | 현재 가격의 트리거 시간; 다음 관측 시작 시간은 이 논리를 기반으로 계산됩니다. |
| feeRate                      | object         | 거래 및 정산 수수료율 (선택 사항)                        |
| leverageInfo                 | object         | 대출 정보 (선택 사항)                        |
| relevantDollarPrices         | list[object]        | RCH 가격 변환 및 에어드랍 계산에 필요한 토큰 가격 (선택 사항)                       |
| amounts                      | object         | 계산된 금액 (선택 사항)                       |
| apyInfo                      | object         | 연환산 정보, 비서지 제품에 대해 제공 (선택 사항)                     |
| oddsInfo                     | object         | 배당률 정보, 서지 제품에 대해 제공 (선택 사항)                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | 메이커의 담보 금액                                                                         |
| \> totalCollateral            | string       | 총 담보 금액 (테이커 + 메이커)                                                             |
| \> collateralAtRisk | string       | 보장될 때 필요 (선택 사항)                                                           |
| \> makerBalanceThreshold | string       | 메이커 잔액 기준                                                          |
| \> deadline                   | number         | 만료 타임스탬프 (예: 1672387200)                                                           |
| \> makerWallet                | string       | 메이커의 지갑 (선택 사항)                                                                     |
| \> signature                  | string       | 서명 (선택 사항)                                                                          |

**요청 예시**

```
GET rfq/smart-trend/recommended-list?vault=xxxxxx
```

### 스마트 트렌드 조회

- 주의 사항:
  - 순수 조회 요청 시 사용자의 지갑 주소를 전달하지 마십시오.
  - 사용자의 지갑 주소는 구독 시에만 전달해야 합니다.

```
GET /rfq/smart-trend/quote
```

**입력 매개변수**

| **필드 이름**     | **필요 여부** | **유형** | **설명**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | 계약 정보                                                                                                   |
| chainId            | true         | int      | 체인 ID                                                                                                               |
| expiry             | true         | number     | 만료 날짜의 초 단위 타임스탬프, 예: 1672387200                                                           |
| lowerBarrier       | true         | number   | 하한 가격                                                                                                            |
| upperBarrier       | true         | number   | 상한 가격                                                                                                            |
| depositAmount      | true         | number   | RFQ 구매 금액                                                                                                    |
| inputApyDefinition | true         | string   | 입력 APY가 계산되는 방법을 나타내는 Enum: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | 보장된 연간 수익률 (위험한 경우 비워두고, 보호된 경우 필요)                                                      |
| fundingApy         | false        | number   | AAVE 연간 수익률 (위험한 경우 비워두고, 보호된 경우 필요)                                                            |
| takerWallet        | false        | string   | 문의자의 지갑 공개 주소 정보                                                                           |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                      |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0은 반환 결과가 정상임을 나타냅니다              |
| message        | string       | 예외 발생 시 반환되는 오류 메시지               |
| value          | object       | 아래와 같이                                   |

객체

| **필드 이름**               | **유형**     | **설명**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | 계약 주소                                                                                  |
| chainId                      | int          | 체인 ID                                                                                          |
| riskType                     | string       | 위험 유형: PROTECTED, RISKY                                                                       |
| direction                    | string       | BULLISH, BEARISH | 
| forCcy                       | string       | 기초 통화                                                                                   |
| domCcy                       | string       | 통화 쌍                                                                                     |
| depositCcy                   | string       | 구독 통화                                                                                   |
| lowerBarrier                 | number       | 하한 가격                                                                                   |
| upperBarrier                 | number       | 상한 가격                                                                                   |
| depositAmount                | number       | RFQ 구매 금액                                                                               |
| expiry                       | number       | 만료 타임스탬프 (예: 1672387200)                                                               |
| timestamp                    | number       | 현재 가격 책정의 트리거 시간; 다음 관측 시작 시간은 이 논리를 기반으로 계산됩니다          |
| feeRate                      | object       | 거래 및 결제 수수료율 (선택 사항)                        |
| leverageInfo                 | object       | 대출 정보 (선택 사항)                        |
| relevantDollarPrices         | list[object] | RCH 가격 변환 및 에어드랍 계산에 필요한 토큰 가격 (선택 사항)                       |
| amounts                      | object       | 계산된 금액 (선택 사항)                       |
| apyInfo                      | object       | 연간화 정보, 비 서지 제품에 대해 사용 가능 (선택 사항)                     |
| oddsInfo                     | object       | 확률 정보, 서지 제품에 대해 사용 가능 (선택 사항)                    |
| quote                        | object       |                                                                                                   |
| \> quoteId                  | number       |                                                                           |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral           | string       | 메이커의 담보 금액                                                                         |
| \> totalCollateral           | string       | 총 담보 금액 (테이커 + 메이커)                                                             |
| \> collateralAtRisk          | string       | 보장이 필요할 때 (선택 사항)                                                           |
| \> makerBalanceThreshold      | string       | 메이커 잔액 기준                                                          |
| \> deadline                   | number       | 만료 타임스탬프 (예: 1672387200)                                                           |
| \> makerWallet                | string       | 메이커의 지갑 (선택 사항)                                                                     |
| \> signature                  | string       | 서명 (선택 사항)                                                                          |

### 스마트 트렌드 승리 확률

만료 시까지 경계 내에 머물 확률

```
GET rfq/smart-trend/winning-probabilities
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | 기초 통화      |
| expiry          | true         | number     | 만료 날짜에 대한 2차 타임스탬프, 예: 1672387200 |
| lowerBarrier       | true         | number   | 하한 가격 |
| upperBarrier       | true         | number   | 상한 가격 |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0은 반환 결과가 정상임을 나타냅니다   |
| message        | string       | 예외 발생 시 반환되는 오류 메시지 |
| value          | list[object] | 아래와 같이                                 |

객체

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | number | 현물 가격 |
| timestamp | number |  |
| probabilities | object | 승리 확률  |


**요청 예시**

```
GET rfq/smart-trend/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerStrike=xxx&upperStrike=xxx
```

**응답**

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

## 일반 인터페이스

### RFQ 삭제

온체인에 있는 RFQ를 삭제합니다.

```
POST rfq/remove
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| rfqId          | true         | number       | .       |

**요청 예시**

```
{  
"rfqId":123456  
}
```

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0은 반환 결과가 정상임을 나타냅니다.   |
| message        | string       | 예외 발생 시 반환되는 오류 메시지 |
| value          | object       | .                               |


### 거래 알림

```
POST rfq/trade
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| quotes | true | list[object] |   |
| \> rfqId | true | number |     |
| \> quoteId | true | number |     |
| \> txId | true | string | 거래 해시 |
| code | false | string | 초대 코드 |
| walletType | false | string | MetaMask, OKX Wallet, Coinbase 등과 같은 지갑 유형 |

**요청 예시**

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

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                    |
| -------------- | ------------ | ------------------------------------------ |
| code           | int          | 0은 반환 결과가 정상임을 나타냅니다.       |
| message        | string       | 예외 발생 시 반환되는 오류 메시지          |
| value          | object       | .                                         |

### 스트라이크 목록

```
GET rfq/strike-list
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| indexPrice | true | number |  |
| forCcy | false | string | BTC-USDT 거래 쌍의 경우, 여기에는 BTC를 입력하십시오. 제공되지 않으면 기본 구성이 사용됩니다. |
| domCcy | false | string | BTC-USDT 거래 쌍의 경우, 여기에는 USDT를 입력하십시오. 제공되지 않으면 기본값은 USDT입니다. |


**요청 예시**

```
{  
"indexPrice": 3750.8，  
"forCcy": "WBTC",  
}
```

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| strikes | list[number] | 기본 추천 행사가격 목록 |

### 만료 목록

```
GET rfq/expiry-list
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| vault | true | string | 계약 주소 |
| chainId | true | int | 체인 ID |


**요청 예시**

```
{  
"vault": "XXXXXXXXXXXXXX",  
"chainId": 1,  
}
```

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| timestamp | number | 기준 시작 시간(초 단위 타임스탬프) |
| expiries | list[number] | 지원되는 만료 목록(초 단위 타임스탬프), 예: 1672387200 |

### Aave apy

```
GET rfq/aave-apy
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | 체인 ID |
| ccy | string | 입금 통화 |
| avgApy | string | 지난 30일 동안 온체인에서 기록된 평균 APY |
| currentApy | string | 최신 AAVE APY |
| apyUsed | string | 미래 이자 수익을 추정하기 위해 SofaServer에서 실제로 사용된 APY |
| apyDefinition | string | APY에 해당하는 계산 정의 |

**응답 예시**

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

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |
| apyDefinition | false | string | APY에 해당하는 계산 정의; 기본값은 AaveLendingApy |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | 체인 ID |
| ccy | string | 예치 통화 |
| avgApy | string | 지난 30일 동안 온체인에 기록된 평균 APY |
| currentApy | string | 최신 APY |
| apyUsed | string | 미래 이자 수익을 추정하기 위해 SofaServer에서 실제로 사용된 APY |
| apyDefinition | string | APY에 해당하는 계산 정의 |

**응답 예시**

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

### 지갑 포지션 가져오기

```
POST /rfq/position-list
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 계약 주소의 집합; 제공되지 않으면 모든 계약이 조회됩니다. |
| claimed | false | boolean | 상환되었는지 여부; 제공되지 않으면 모든 상태의 포지션이 조회됩니다. |
| expired | false | boolean | 만료되었는지 여부; 제공되지 않으면 모든 상태의 포지션이 조회됩니다. |
| concealed | false | boolean | 숨겨져 있는지 여부; 제공되지 않으면 모든 상태의 포지션이 조회됩니다. |
| positiveReturn | false | boolean | 상환 금액이 0보다 큰지 여부; 제공되지 않으면 모든 포지션이 조회됩니다. |
| positiveProfit | false | boolean | 수익이 원금을 초과하는지 여부; 제공되지 않으면 모든 포지션이 조회됩니다. |
| limit | false | Int | 조회 수; 기본값은 100, 최대값은 300입니다. |
| startDateTime | false | number | 해당하는 타임스탬프(초 단위, 포함), 예: 1672387200. |
| endDateTime | false | number | 해당하는 타임스탬프(초 단위, 포함), 예: 1672387200. |
| orderBy | false | string | "createdAt" 또는 "return", 정렬 방법: "createdAt" (업데이트 시간, 기본값) 또는 "return" (수익). |
| orderDirection | false | string | "desc" 또는 "asc", 기본값은 "desc" (내림차순)입니다. |
| wallet | false | string | 지갑 주소 (비어 있으면 모든 지갑 주소가 조회됩니다). |

참고: 지정된 `startDateTime` 및 `endDateTime` 내에서 결과 수가 300을 초과하면 폴링을 사용할 수 있습니다(이 경우 `orderBy`는 `"createdAt"`으로 설정해야 함). n번째 폴링 요청의 `endDateTime` 매개변수는 (n-1)번째 폴링 결과의 마지막 레코드의 `createdAt`으로 설정해야 합니다. 모든 데이터를 검색한 후에는 `id` 필드를 기준으로 중복을 제거해야 합니다.

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 아래와 같이 |

객체

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string | 포지션에 해당하는 제품 ID (`The Graph` 데이터의 `productId`와 일치). 이 ID는 체인 + 금고 차원 내에서 고유하며 포지션 잔액을 조회하는 데 사용될 수 있습니다. |
| positionId | string | 체인 차원 내에서 고유한 포지션 ID. |
| product | object | 제품 정보. |
| wallet | string | 지갑 주소. |
| createdAt | number | 해당하는 타임스탬프(초 단위), 예: 1672387200. |
| updatedAt | number | 해당하는 타임스탬프(초 단위), 예: 1672387200. |
| claimed | boolean | 포지션이 상환되었는지 여부. |
| takerAllocationRate | number | 계산 시간에 따라 소유자가 베팅 풀에서 가져갈 수 있는 비율을 추정합니다. 만료 전에는 추정 값입니다. |
| triggerTime | number | 범위 제한의 경우 첫 번째 돌파 시간; 비범위 제한의 경우 정산 시간(초 단위). |
| triggerPrice | number | 범위 제한의 경우 첫 번째 돌파 가격; 비범위 제한의 경우 정산 가격. |
| feeRate | object | 수수료 비율. |
| leverageInfo | object | 대출 정보 (선택 사항). |
| relevantDollarPrices | list[object] | RCH 가격 변환 및 에어드랍 계산에 필요한 토큰 가격. |
| amounts | object | 계산된 금액. |
| apyInfo | object | 연간화된 정보, Surge 제품이 아닌 경우에 사용 가능. |
| oddsInfo | object | 확률 정보, Surge 제품에 대해 사용 가능. |
| claimParams | object | 청구 매개변수 정보. |

**응답 예시**

```
{
    "value": [{
        ...
    }]
}
```

### 지갑 거래 내역 가져오기

```
POST /rfq/transaction-list
```

**입력 매개변수**

| **필드 이름** | **필요 여부** | **유형** | **설명** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 계약 주소 집합; 제공되지 않으면 모든 계약이 조회됩니다. |
| limit | false | Int | 쿼리 수; 기본값은 100, 최대값은 300입니다. |
| startDateTime | false | number | 해당하는 타임스탬프(초 단위, 포함), 예: 1672387200. |
| endDateTime | false | number | 해당하는 타임스탬프(초 단위, 포함), 예: 1672387200. |
| orderDirection | false | string | "desc" 또는 "asc", 기본값은 "desc"(내림차순)입니다. |
| taker | false | string | 테이커 지갑 주소 |
| maker | false | string | 메이커 지갑 주소 |
| claimParams | false | object | 청구 매개변수, 포지션 목록 데이터의 claimParams 필드에 해당합니다. |
| hash | false | string | 거래 해시 |

참고: 테이커, 메이커 및 claimParams 매개변수는 포지션에 해당하는 거래 기록을 찾기 위한 공동 쿼리에 사용될 수 있습니다(포지션은 여러 거래에 해당할 수 있음)(모든 매개변수가 필요하지는 않음). 지정된 `startDateTime` 및 `endDateTime` 내에서 결과 수가 300을 초과하는 경우, 폴링을 사용할 수 있습니다(이 경우 `orderBy`는 `"createdAt"`으로 설정해야 함) 계속 쿼리합니다. n번째 폴링 요청의 `endDateTime` 매개변수는 (n-1)번째 폴링 결과의 마지막 기록의 `createdAt`으로 설정해야 합니다. 모든 데이터를 가져온 후에는 `id` 필드를 기준으로 중복을 제거해야 합니다.

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 아래와 같이 |

객체

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | 거래 해시 |
| takerWallet | string | 테이커 지갑 주소. |
| makerWallet | string | 메이커 지갑 주소. |
| product | object | 제품 정보. |
| createdAt | number | 초 단위의 해당 타임스탬프, 예: 1672387200. |
| takerAllocationRate | number | 계산 시점을 기준으로 소유자가 베팅 풀에서 가져갈 수 있는 비율을 추정합니다. 만료 전에는 추정 값입니다. |
| triggerTime | number | 범위 제한의 경우 첫 번째 돌파 시간; 비범위 제한의 경우 정산 시간, 초 단위. |
| triggerPrice | number | 범위 제한의 경우 첫 번째 돌파 가격; 비범위 제한의 경우 정산 가격. |
| feeRate | object | 수수료 비율. |
| leverageInfo | object | 대출 정보 (선택 사항). |
| relevantDollarPrices | list[object] | RCH 가격 변환 및 에어드랍 계산에 필요한 토큰 가격. |
| amounts | object | 계산된 금액. |
| apyInfo | object | 연간화 정보, 비서지 제품에 대해 제공됩니다. |
| oddsInfo | object | 배당률 정보, 서지 제품에 대해 제공됩니다. |

**응답 예시**

```
{
    "value": [{
        ...
    }]
}
```

### 손실 포지션 숨기기

```
POST rfq/position/conceal
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| chainId | true | int | 체인 ID |
| positionIds | true | List[string] | positionId 목록, 최대 20개 |

**요청 예시**

```
{
    "positionIds": [
        "aaaa","bbbb"
    ],
    "chainId": 1
}
```

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0은 반환 결과가 정상임을 나타냅니다   |
| message        | string       | 예외 발생 시 반환되는 오류 메시지 |
| value          | object       | .                               |


### 보류 중인 거래 가져오기

이는 주로 기본 포지션 데이터 동기화 문제를 해결하기 위한 것입니다. 여기서 검색된 데이터는 사라질 수 있습니다 (블록체인 포크 경쟁으로 인해).

```
POST rfq/transactions/pending
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | 계약 주소 집합; 제공되지 않으면 모든 계약이 조회됩니다. |
| taker | false | string | 테이커 지갑 주소 |
| maker | false | string | 메이커 지갑 주소 |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 아래와 같이 표시됩니다. |

객체

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | 거래 해시 |
| takerWallet | string | 테이커 지갑 주소. |
| makerWallet | string | 메이커 지갑 주소. |
| product | object | 제품 정보. |
| createdAt | number | 초 단위의 해당 타임스탬프, 예: 1672387200. |
| takerAllocationRate | number | 계산 시점을 기준으로 소유자가 베팅 풀에서 가져갈 수 있는 비율을 추정합니다. 만료 전에는 추정값입니다. |
| triggerTime | number | 범위 제한의 경우 첫 번째 돌파 시간; 비범위 제한의 경우 정산 시간, 초 단위. |
| triggerPrice | number | 범위 제한의 경우 첫 번째 돌파 가격; 비범위 제한의 경우 정산 가격. |
| feeRate | object | 수수료 비율. |
| leverageInfo | object | 대출 정보 (선택 사항). |
| relevantDollarPrices | list[object] | RCH 가격 변환 및 에어드롭 계산에 필요한 토큰 가격. |
| amounts | object | 계산된 금액. |
| apyInfo | object | 연간화 정보, 비서지 제품에 대해 제공됩니다. |
| oddsInfo | object | 배당률 정보, 서지 제품에 대해 제공됩니다. |

### RCH 에어드롭 기록 가져오기

```
GET rfq/airdrop/history
```

**입력 매개변수**

| **필드 이름** | **필수** | **유형** | **설명** |
| --- | --- | --- | --- |
| wallet | true | string | 지갑 주소 |
| startDateTime | true | number | 해당 타임스탬프(초 단위, 포함), 예: 1672387200. |
| endDateTime | true | number | 해당 타임스탬프(초 단위, 포함), 예: 1672387200. |
| orderBy | false | string | "dateTime" 또는 "rch", 정렬 방법: "dateTime" (에어드랍 시간, 기본값) 또는 "rch" (금액). |
| orderDirection | false | string | "desc" 또는 "asc", 기본값은 "desc" (내림차순). |

**응답 매개변수**

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | 아래와 같이 |

객체

| **필드 이름** | **유형**     | **설명**                                |
| -------------- | ------------ | ---------------------------------------------- |
| dateTime | long | 현재 타임스탬프(초 단위), 예: 1672387200. |
| wallet | string | 지갑 주소 |
| volume | string | 거래량 |
| rch | string | 에어드랍 금액, 원시 값, 1e18로 나누어야 함 |
| merkleProof | string | 머클 증명 |
