# 온체인 정산 고려사항

## 정산 물류

> **DeFi 세계의 작업 스케줄링 문제**

전통적인 시스템에서는 사소한 문제인 **시간 예약 데이터 검색은 블록체인에서 도전적입니다. 스마트 계약에는 기본적인 'Cron과 유사한' 작업 스케줄러가 없기 때문입니다**. 암호화폐에서 가격이 제때 예약되고 업데이트되도록 보장하는 자동화 메커니즘을 어떻게 설계할 수 있을까요?

Basis Cash나 ESD와 같은 암호화 '알고 스테이블'은 DeFi 부문을 휩쓸며 각 에포크의 끝에서 작업을 진행하고 새로운 정산 가격을 설정하기 위해 _수동_ 트리거가 필요한 '에포크' 간격 접근 방식을 배포했습니다. **보상 메커니즘은 자원봉사자들이 '진행을 트리거'하도록 강제하기 위해 스마트 계약 코드에 내장되었습니다**. 비슷하게, Andre Cronje(Yearn Finance, Fantom)가 설립한 인기 프로젝트인 Keep3r는 이 특정 목적을 위해 사전 요청을 게시하고 지정된 작업 보상을 제공하는 가상 작업 게시판을 만들었습니다.

어느 쪽 솔루션도 완벽하지 않았습니다. 왜냐하면 **궁극적으로 인간의 개입에 의존했기 때문입니다**(자기 이익에 의해 동기 부여됨). 수동 노력이 필요하다는 점에서 **실제 최종 실행 시간에 대한 불확실성**을 언급할 필요도 없습니다. 이는 정산 고정이 만료 후에도 업데이트되지 않는 불편한 상황을 초래할 수 있으며, 이는 사용자 경험을 불만족스럽게 만들 수 있습니다.

가격 업데이트를 주기적으로 트리거하는 오프체인 솔루션을 만들고 의존할 수 있을까요? 가능하지만, **프로토콜이 서버 장애와 같은 위험으로 심각하게 손상될 수 있는 중앙 집중식 '지름길'에는 관심이 없습니다**.

우리의 오라클 섹션에서 다룬 바와 같이, SOFA는 **ChainLink의 자동화 서비스를 정산 가격 소스로 활용합니다**. 그들의 서비스는 신뢰할 수 있고 분산된 자동화 플랫폼을 통해 스마트 계약 기능의 조건부 실행을 가능하게 하며, 현재 수십억 달러의 TVL을 확보한 외부 노드 운영의 검증된 네트워크를 가지고 있습니다.

마지막으로, **최종 정산 가격이 설정되면 사용자와 시장 조성자는 언제든지 계약을 호출하여 자신의 포지션 토큰을 소각하고 정당한 지급금을 받을 수 있습니다**, 이는 SOFA의 분산 오라클에서 신뢰할 수 있는 데이터 피드를 통해 스마트 계약이 원활하게 처리합니다.

## 지급 계산

제품 지급을 계산하는 계약은 완전히 표준화되어 있으며, Vault에 독립적이고 담보에 구애받지 않습니다. 대신, **계산 계약은 모델 지급을 계산하기 위해 기본 구조화된 제품의 유형에 의해 정의됩니다**. 몇 가지 지급 예시는 아래에 나와 있습니다:

### _범위 제한_

- $$Payoff{maker}=\begin{cases}X, HighSettlePrice\geq HighStrikePrice\vee LowSettlePrice\leq LowStrikePrice\\  0, HighSettlePrice<HighStrikePrice\wedge LowSettlePrice>LowStrikePrice\end{cases}$$
- $$Payoff {user}=X - Payoff {maker}$$

### _스마트 트렌드_

#### _강세 트렌드_

- $$Payoff{maker}=\begin{cases}0, SettlePrice\geq HighStrikePrice\\
X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
X, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

#### _약세 트렌드_

- $$Payoff{maker}=\begin{cases}X, SettlePrice\geq HighStrikePrice\\

X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
0, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$