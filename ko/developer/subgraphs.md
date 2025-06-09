# 서브그래프

우리는 [The Graph](https://thegraph.com/)'의 인덱싱 프로토콜을 통해 실시간으로 정확한 블록체인 데이터 접근을 제공함으로써 효율적이고 반응성이 뛰어난 애플리케이션 개발을 지원합니다. 서브그래프는 이더리움에 저장된 데이터를 인덱싱하고 이 데이터를 GraphQL을 통해 쿼리할 수 있게 해주는 강력한 도구입니다.

다음은 프로토콜 내에서 관련 데이터를 쿼리하기 위해 사용자 정의 서브그래프를 활용하는 방법입니다.

| 서브그래프                | 쿼리 URL                                  |
|-------------------------|--------------------------------------------|
| SOFA 메인넷            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5Po2c7F3DiGty1pCRpsDF9yURbpapiWXmkw9ckbafLqe |
| SOFA 아비트럼           | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/HcQUG7TbdiSUpsNd4QxJ54iAHvD4TjmkUxsTfkgFdhmC |
| SOFA BSC                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/88UgiNTsJjJ15V1GXTRnUxJBza3ZsrYZyUdAiVuRwQbX |
| SOFA 폴리곤            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5AyRj7tY5HsXznUBCQMKuVbbdcBXQfSRQ5K77wMBwER1 |
| SOFA 세이                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/9NTKYrnPsZASfbe8gx55ZqmViWLwEZNArbkQbC6cXRVb |
| SOFA 오토메이터 메인넷  | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/Ao7xxFupmSqH8imXCCLKK8KJnBwkMrTrkGtFfP78Mqr |
| SOFA 오토메이터 아비트럼 | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/7DKnoe1Hqek8BWrmMFKJF6RfNTH9z8th7yHqM7MCYjCt |

## 우리의 서브그래프 접근하기

우리는 프로토콜의 데이터를 위한 특정 서브그래프를 생성했습니다. 이 서브그래프는 거래, 계정 및 우리의 DeFi 제품과 관련된 특정 이벤트와 같은 블록체인에서 인덱싱된 다양한 엔티티를 포함하고 있습니다. 다음 단계에 따라 서브그래프에서 데이터를 쿼리할 수 있습니다:

### 단계 1: 엔드포인트 찾기

상호작용하고자 하는 서브그래프에 해당하는 GraphQL 엔드포인트 URL을 찾습니다. 이는 일반적으로 웹 주소 링크로 나타납니다. 최신 엔드포인트를 사용하고 있는지 확인하십시오.

### 단계 2: 스키마 이해하기

스키마는 서브그래프 내의 엔티티와 수행할 수 있는 쿼리의 유형을 설명합니다. 스키마를 이해하는 것은 효율적인 쿼리를 작성하는 데 중요합니다.

### 단계 3: GraphQL 쿼리 작성하기

스키마를 기반으로 서브그래프에서 데이터를 쿼리하기 위해 GraphQL 쿼리를 작성하기 시작할 수 있습니다. GraphQL은 관심 있는 필드만 요청하는 매우 구체적인 쿼리를 허용합니다. 예를 들어, 각 거래에 대한 자세한 정보를 쿼리하고 싶다면 다음과 같은 쿼리를 작성할 수 있습니다:

```
{
  transactions(first: 5) {
    vault
    tradingFeeRate
    totalCollateral
    timestamp
    term
    spreadAPR
    referral
    minter
    makerCollateral
    maker
    id
    hash
    expiry
    collateralAtRiskPercentage
    borrowAPR
    anchorPrices
  }
}
```

이것은 블록체인에서 첫 번째 다섯 개 기록에 대한 거래 및 관련 세부 정보를 반환합니다.

### 4단계: 쿼리 요청 보내기

GraphQL을 지원하는 HTTP 클라이언트를 사용하여 쿼리 요청을 보내십시오. curl과 같은 명령줄 도구를 사용하거나, apollo-client와 같이 애플리케이션에 통합된 라이브러리를 사용할 수 있습니다. 일반적으로 요청은 POST 요청으로 엔드포인트 URL에 전송됩니다.

curl을 사용하는 예제 코드는 다음과 같습니다:

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

실제 상황에 따라 URL을 교체하십시오.

### 5단계: 응답 분석 및 사용

쿼리 요청이 전송된 후, GraphQL 서비스는 JSON 형식으로 응답을 반환합니다. 이 데이터를 애플리케이션에서 처리하거나 프론트 엔드에서 사용자에게 직접 표시할 수 있습니다.

## 모범 사례

- **쿼리 변수 사용**: 동일한 구조의 쿼리를 자주 수행해야 하지만 다른 매개변수를 사용하는 경우, 쿼리 변수를 사용하면 쿼리를 재사용하기가 더 쉬워집니다.
- **오류 처리**: 정상적인 응답 외에도 GraphQL은 오류 정보를 반환할 수 있습니다. 클라이언트 코드가 이러한 상황을 우아하게 처리할 수 있도록 하십시오.

위 단계를 따르면 SOFA 프로토콜에서 데이터를 쿼리하여 개발 프로젝트를 지원하기 위해 The Graph를 효과적으로 사용할 수 있습니다. 추가 정보는 나머지 개발자 문서 및 서브그래프 관련 리소스를 참조하십시오.
