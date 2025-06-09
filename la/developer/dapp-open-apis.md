# DApp Open APIs

Nostrum existentem dApp in sofa.org est critica workflow feature et est principalis modus pro usoribus ad interagendum cum SOFA protocollis. Tamen, alios progressus hortamur ut accessum enableant et se coniungant ad SOFA per suas proprias dApps ut maximam nostram ecosystematis incrementum. Consideramus hos esse nostros 'Broker' socios et hortamur partes interestas ut contactum cum SOFA turma pro ulteriori API informatione.

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
| code           | int          | 0 indicat quod resultatum redditum normale est   |
| message        | string       | Error message redditum in casu exceptionis |
| value          | list[object] | Ut infra demonstratum                                               |

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
| depositAmount                | number       | RFQ emptio quantitatis                                                                               |
| expiry                       | number         | Expiratio temporis (e.g., 1672387200)                                                               |
| timestamp                    | number         | Tempus incitamentum praesentis pretium; proxima observatio initium tempus ex hoc logica calculatur |
| observationStart             | number         | Tempus initium observationis aestimatum pro pulsando in/out ex temporis signo                        |
| feeRate             | object         | mercaturae et solutionis feodum rate (optio)                        |
| leverageInfo             | object         | Informationes de mutuo (optio)                        |
| relevantDollarPrices             | list[object]         | Pretium token necessarium pro RCH pretium conversione et airdrop calculatione (optio)                       |
| amounts             | object         | Calculatae quantitates (optio)                       |
| apyInfo             | object         | Annuae informationes, praesto pro non-Surge productis (optio)                     |
| oddsInfo             | object         | Informationes de probabilitate, praesto pro Surge productis (optio)                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Collateralis maker quantitas                                                                         |
| \> totalCollateral            | string       | Totalis collateralis quantitas (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Necessarium cum cautum (optio)                                                           |
| \> makerBalanceThreshold | string       | maker aequilibrium limitem                                                          |
| \> deadline                   | number         | Expiratio temporis (e.g., 1672387200)                                                           |
| \> makerWallet                | string       | Fenestra maker (optio)                                                                     |
| \> signature                  | string       | Signatura (optio)                                                                          |

**Request Exemplum**

```
GET rfq/dnt/recommended-list?vault=xxxxxx&chainId=1
```

**Responsio**

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
  - Quaeso ne transmittas inscriptionem nummariae usoris in pura inquisitione petitione.
  - Inscriptionem nummariae usoris tantum transmittenda est cum subscripsione.

```
GET /rfq/dnt/quote
```

**Input Parameters**

| **Field Name**     | **Required** | **Type** | **Description**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | verum        | string   | Informatio contractus                                                                                                   |
| chainId            | verum        | int      | ID catena                                                                                                               |
| expiry             | verum        | number   | Tempus secundarium pro die expirationis, e.g., 1672387200                                                           |
| lowerBarrier       | verum        | number   | Pretium inferius                                                                                                        |
| upperBarrier       | verum        | number   | Pretium superius                                                                                                        |
| depositAmount      | verum        | number   | Quantitas emptionis RFQ                                                                                               |
| inputApyDefinition | verum        | string   | Codex subiacens est Enum indicans quomodo input APY computetur: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | falsum       | number   | Certum annuum fructum (vacuum pro RISKY, necessarium pro protecto)                                                  |
| fundingApy         | falsum       | number   | AAVE annuum fructum (vacuum pro RISKY, necessarium pro protecto)                                                    |
| takerWallet        | falsum       | string   | Informatio publica inscriptionis nancientis                                                                           |

**Response Parameters**

| **Nomen Campi** | **Typus**    | **Descriptio**                                |
| --------------- | ------------ | ---------------------------------------------- |
| code            | int          | 0 indicat quod resultatum reditus normale est   |
| message         | string       | Nuntius erroris redditus in casu exceptionis |
| value           | object       | Ut infra demonstratum                                 |

Object

| **Nomen Campi**               | **Typus**    | **Descriptio**                                                                                   |
| ----------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                         | number       | ID RFQ                                                                                |
| vault                         | string       | Inscriptionis contractus                                                                                  |
| chainId                       | int          | ID catena                                                                                          |
| riskType                      | string       | Typus periculi: PROTECTUS, RISKY                                                                       |
| forCcy                        | string       | Moneta subiacens                                                                               |
| domCcy                        | string       | Parium monetarum                                                                                     |
| depositCcy                    | string       | Moneta subscriptionis                                                                             |
| lowerBarrier                  | number       | Pretium inferius                                                                                       |
| upperBarrier                  | number       | Pretium superius                                                                                       |
| depositAmount                 | number       | Quantitas emptionis RFQ                                                                               |
| expiry                        | number       | Tempus expirationis (e.g., 1672387200)                                                               |
| timestamp                     | number       | Tempus incitamentum pretiorum currentium; tempus initii observationis sequentis ex hoc logica computatur |
| observationStart              | number       | Tempus initii aestimationis observationis pro knocking in/out secundum tempus                        |
| feeRate                       | object       | Rationem feorum negotiationis et solutionis (optativa)                        |
| leverageInfo                  | object       | Informatio de mutuo (optativa)                        |
| relevantDollarPrices          | list[object] | Pretium tokenis necessarium pro conversione pretii RCH et calculo airdrop (optativa)                       |
| amounts                       | object       | Quantitates computatae (optativa)                       |
| apyInfo             | object         | Informatio annua, praesto pro productis non-Surge (optativa)                     |
| oddsInfo             | object         | Informatio de probabilitatibus, praesto pro productis Surge (optativa)                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Quantitas collateralis Maker                                                                         |
| \> totalCollateral            | string       | Totalis quantitas collateralis (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Necessaria cum cautum (optativa)                                                           |
| \> makerBalanceThreshold | string       | Limite aequilibrii Maker                                                          |
| \> deadline                   | number         | Tempus expirationis (e.g., 1672387200)                                                           |
| \> makerWallet                | string       | Nummorum sacculus Maker (optativa)                                                                     |
| \> signature                  | string       | Signatura (optativa)                                                                          |

### DNT Probabilitas Vincendi

Probabilitas manendi intra limites usque ad expirationem

```
GET rfq/dnt/winning-probabilities
```

**Input Parameters**

| **Nomen Campi** | **Requisitum** | **Typus** | **Descriptio** |
| --- | --- | --- | --- |
| forCcy          | verum         | string       | Moneta subiecta      |
| expiry          | verum         | number     | Tempus secundarium pro die expirationis, e.g., 1672387200 |
| lowerBarrier       | verum         | number   | Inferior pretium |
| upperBarrier       | verum         | number   | Superior pretium |

**Response Parameters**

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicat quod resultatum reditionis normale   |
| message        | string       | Nuntius erroris redditus in casu exceptionis |
| value          | list[object] | Ut infra demonstratum                                 |

Object

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | numerus | Pretium locatum |
| timestamp | numerus |  |
| probabilities | objectum | Probabilitas vincendi  |

**Exemplum Postulationis**

```
GET rfq/dnt/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerBarrier=xxx&upperBarrier=xxx
```

**Responsio**

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

## Callidus Trend

### Consilium Callidi Trend RFQ indicium

```
GET rfq/smart-trend/recommended-list
```

**Input parameters**

| **Field Name**       | **Required**  | **Type** | **Description**                                         |
| -------------------- | ------------- | -------- | ------------------------------------------------------------ |
| vault              | verum         | string   | Notitia contractus                               |
| chainId            | verum         | int      | ID catena                    |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicat quod resultatum reditus normale   |
| message        | string       | Nuntius erroris redditus in casu exceptionis |
| value          | list[object] | Ut infra demonstratum                                 |

Object

| **Field Name**               | **Type**     | **Description**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| chainId                      | int          | ID catena                                                                                          |
| vault                        | string       | Adres contractus                                                                                  |
| riskType                     | string       | Typus periculi: PROTECTUS, PERICULOSUS                                                                       |
| direction                    | string       | BULLISH, BEARISH |   
| forCcy                       | string       | Moneta subiecta                                                                               |
| domCcy                       | string       | Parium monetarum                                                                                     |
| depositCcy                   | string       | Moneta subscripta                                                                             |
| lowerBarrier                 | number       | Inferior pretium                                                                                       |
| upperBarrier                 | number       | Superior pretium                                                                                       |
| depositAmount                | number       | Quantitas emptionis RFQ                                                                               |
| expiry                       | number         | Tempus expirationis (e.g., 1672387200)                                                               |
| timestamp                    | number         | Tempus incitamenti pretiorum currentium; tempus initii observationis sequentis ex hoc logica calculatur |
| feeRate                      | object         | Rationem mercedis negotiationis et solutionis (optativa)                        |
| leverageInfo                 | object         | Notitia de mutuo (optativa)                        |
| relevantDollarPrices         | list[object]  | Pretium tokenis necessarium pro conversione pretii RCH et calculatione airdrop (optativa)                       |
| amounts                      | object         | Quantitates calculatae (optativa)                       |
| apyInfo                      | object         | Notitia annualis, praesto pro productis non-Surge (optativa)                     |
| oddsInfo                     | object         | Notitia de probabilitatibus, praesto pro productis Surge (optativa)                    |
| quote                        | object       |                                                                                                   |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Quantitas collateralis Maker                                                                         |
| \> totalCollateral            | string       | Totalis quantitas collateralis (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Necessarium cum cautum (optativum)                                                           |
| \> makerBalanceThreshold | string       | Limitem aequilibrii Maker                                                          |
| \> deadline                   | number         | Tempus expirationis (exempli gratia, 1672387200)                                                           |
| \> makerWallet                | string       | Fiscus Maker (optativum)                                                                     |
| \> signature                  | string       | Signatura (optativum)                                                                          |

**Exemplum Petitionis**

```
GET rfq/smart-trend/recommended-list?vault=xxxxxx
```

### Inquisitio de Trend Intelligent

- Notae:
  - Quaeso ne transmittas inscriptionem fisco usoris in pura petitione inquisitionis.
  - Inscriptionem fisco usoris tantum transmitti debet cum subscripto.

```
GET /rfq/smart-trend/quote
```

**Parametri Input**

| **Nomen Campi**     | **Necessarium** | **Typus** | **Descriptio**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | verum         | string   | Notitia contractus                                                                                                   |
| chainId            | verum         | int      | ID catenae                                                                                                               |
| expiry             | verum         | number     | Tempus expirationis in secundis, exempli gratia, 1672387200                                                           |
| lowerBarrier       | verum         | number   | Inferior pretium                                                                                                            |
| upperBarrier       | verum         | number   | Superior pretium                                                                                                            |
| depositAmount      | verum         | number   | Quantitas emptionis RFQ                                                                                                    |
| inputApyDefinition | verum         | string   | Codex subiacens est Enum indicans quomodo input APY calculatur: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | falsum        | number   | Certa annua reditus (vacuum pro RISKY, necessarium pro protecto)                                                      |
| fundingApy         | falsum        | number   | AAVE annua reditus (vacuum pro RISKY, necessarium pro protecto)                                                            |
| takerWallet        | falsum        | string   | Notitia publica inscriptionis fisco inquirentis                                                                           |

**Parametri Responsionis**

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicat quod eventus reditus normalis est   |
| message        | string       | Nuntius erroris redditus in casu exceptionis |
| value          | object       | Ut infra demonstratum est                                 |

Object

| **Nomen Campi**               | **Typus**     | **Descriptio**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ ID                                                                                |
| vault                        | string       | Adressum contractus                                                                                  |
| chainId                      | int          | Chain ID                                                                                          |
| riskType                     | string       | Typus periculi: PROTECTUS, PERICULOSUS                                                                       |
| direction                    | string       | BULLISH, BEARISH | 
| forCcy                       | string       | Moneta subiecta                                                                               |
| domCcy                       | string       | Par monetae                                                                                     |
| depositCcy                   | string       | Moneta subscritionis                                                                             |
| lowerBarrier                 | number       | Inferior pretium                                                                                       |
| upperBarrier                 | number       | Superior pretium                                                                                       |
| depositAmount                | number       | Quantitas emptionis RFQ                                                                               |
| expiry                       | number         | Tempus expirationis (e.g., 1672387200)                                                               |
| timestamp                    | number         | Tempus incitamenti praesentis; tempus initii observationis sequentis ex hoc logica calculatur |
| feeRate             | object         | Rationem mercedis negotiationis et solutionis (optativa)                        |
| leverageInfo             | object         | Notitia de mutuo (optativa)                        |
| relevantDollarPrices             | list[object]         | Pretium tokenis necessarium pro conversione pretii RCH et calculatione airdrop (optativa)                       |
| amounts             | object         | Quantitates calculatae (optativa)                       |
| apyInfo             | object         | Notitia annualis, praesto pro productis non-Surge (optativa)                     |
| oddsInfo             | object         | Notitia de probabilitatibus, praesto pro productis Surge (optativa)                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Quantitas collateralis fabricatoris                                                                         |
| \> totalCollateral            | string       | Totalis quantitas collateralis (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Necessarium cum cautum (optativa)                                                           |
| \> makerBalanceThreshold | string       | Limitem rationis fabricatoris                                                          |
| \> deadline                   | number         | Tempus expirationis (e.g., 1672387200)                                                           |
| \> makerWallet                | string       | Fiscus fabricatoris (optativa)                                                                     |
| \> signature                  | string       | Signatura (optativa)                                                                          |

### Sapienti Trend Vincendi Probabilitas

Probabilitas manendi intra fines usque ad expirationem

```
GET rfq/smart-trend/winning-probabilities
```

**Input Parametri**

| **Nomen Campi** | **Required** | **Typus** | **Descriptio** |
| --- | --- | --- | --- |
| forCcy          | verum         | string       | Moneta subiecta      |
| expiry          | verum         | numerus     | Timestamp secundi gradus pro die expirationis, e.g., 1672387200 |
| lowerBarrier       | verum         | numerus   | Inferior pretium |
| upperBarrier       | verum         | numerus   | Superior pretium |

**Responsio Parametri**

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicat quod resultatum reditus normale est   |
| message        | string       | Nuntius erroris redditus in casu exceptionis |
| value          | list[object] | Ut infra demonstratum                                 |

Objectum

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | numerus | Pretium spot |
| timestamp | numerus |  |
| probabilities | object | Probabilitas vincendi  |


**Exemplum Petitionis**

```
GET rfq/smart-trend/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerStrike=xxx&upperStrike=xxx
```

**Responsum**

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
            "probBearTrendItmUpperStrike": 0.6
        }
    }
}
```

## Interface Generalis

### Delere RFQ

Delere RFQ quod in catena fuit

```
POST rfq/remove
```

**Parametri Input**

| **Nomen Campi** | **Requisitum** | **Typus** | **Descriptio** |
| --- | --- | --- | --- |
| rfqId          | verum         | numerus       | .       |

**Exemplum Petitionis**

```
{  
"rfqId":123456  
}
```

**Responsio Parametri**

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicat quod resultatum reditus normale est   |
| message        | string       | Nuntius erroris redditus in casu exceptionis |
| value          | object       | .                               |


### Notificatio Transactionis

```
POST rfq/trade
```

**Parametri Input**

| **Nomen Campi** | **Requisitum** | **Typus** | **Descriptio** |
| --- | --- | --- | --- |
| quotes | verum | list[object] |   |
| \> rfqId | verum | number |     |
| \> quoteId | verum | number |     |
| \> txId | verum | string | Hash transactionis |
| code | falsum | string | Codex invitationis |
| walletType | falsum | string | Typus nummularii, ut MetaMask, OKX Wallet, Coinbase, etc. |

**Exemplum Postulationis**

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

**Responsio Parametri**

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indicat quod eventus reditus normalis est   |
| message        | string       | Nuntius erroris redditus in casu exceptionis |
| value          | object       | .                               |

### Index rerum

```
GET rfq/strike-list
```

**Parametri Input**

| **Nomen Campi** | **Requisitum** | **Typus** | **Descriptio** |
| --- | --- | --- | --- |
| indexPrice | verum | numerus |  |
| forCcy | falsum | string | Pro par BTC-USDT, hic BTC inserere; si non praebetur, configuratio praedefinita adhibebitur. |
| domCcy | falsum | string | Pro par BTC-USDT, hic USDT inserere; si non praebetur, ad USDT revertitur. |


**Exemplum Petitionis**

```
{  
"indexPrice": 3750.8，  
"forCcy": "WBTC",  
}
```

**Responsio Parametri**

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| strikes | list[number] | Default commendatus pretium ictus index |

### Index Expirationis

```
GET rfq/expiry-list
```

**Parametri Input**

| **Nomen Campi** | **Necessarium** | **Typus** | **Descriptio** |
| --- | --- | --- | --- |
| vault | verum | string | Contractus oratio |
| chainId | verum | int | Chain ID |


**Exemplum Postulationis**

```
{  
"vault": "XXXXXXXXXXXXXX",  
"chainId": 1,  
}
```

**Parametri Responsionis**

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| timestamp | number | Baseline initium tempus in secundis timestamp |
| expiries | list[number] | Supportatus index expirationis in secundis timestamp, e.g., 1672387200 |

### Aave apy

```
GET rfq/aave-apy
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | verum | numerus |     |
| ccy | verum | string | USDT |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | numerus | Chain ID |
| ccy | string | Moneta depositi |
| avgApy | string | APY mediocris in catena per praeteritos 30 dies registratum |
| currentApy | string | Novissimum AAVE APY |
| apyUsed | string | APY reapse usus in SofaServer ad aestimandum futurum reditum usurae |
| apyDefinition | string | Definitio calculationis respondens APY |

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

| **Nomen Campi** | **Necessarium** | **Typus** | **Descriptio** |
| --- | --- | --- | --- |
| chainId | verum | numerus |     |
| ccy | verum | string | USDT |
| apyDefinition | falsum | string | Definitio calculationis respondens ad APY; ad AaveLendingApy per defaultum |

**Parametri Responsionis**

| **Nomen Campi** | **Typus**     | **Descriptio**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | numerus | ID Catenae |
| ccy | string | Moneta depositi |
| avgApy | string | APY mediocris in catena per praeteritos 30 dies registrata |
| currentApy | string | Novissimum APY |
| apyUsed | string | APY reapse usus in SofaServer ad aestimandum futurum reditum usurae |
| apyDefinition | string | Definitio calculationis respondens ad APY |

**Exemplum Responsionis**

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

### Accipe Positiones Nummorum

```
POST /rfq/position-list
```

**Parametri Input**

| **Nomen Campi** | **Necessarium** | **Typus** | **Descriptio** |
| --- | --- | --- | --- |
| chainId | verum | Int |  |
| vaults | falsum | list[string] | Set contractuum inscriptionum; si non praebetur, omnia contracta interrogabuntur. |
| claimed | falsum | boolean | Utrum redemptum sit; si non praebetur, positiones omnium status interrogabuntur. |
| expired | falsum | boolean | Utrum exspiravit; si non praebetur, positiones omnium status interrogabuntur. |
| concealed | falsum | boolean | Utrum occultum sit; si non praebetur, positiones omnium status interrogabuntur. |
| positiveReturn | falsum | boolean | Utrum redemptio quantitas maior sit quam 0; si non praebetur, omnes positiones interrogabuntur. |
| positiveProfit | falsum | boolean | Utrum reditus principalem superet; si non praebetur, omnes positiones interrogabuntur. |
| limit | falsum | Int | Numerus interrogationum; default ad 100, maximum est 300. |
| startDateTime | falsum | number | Tempus correspondens in secundis (inclusivum), e.g., 1672387200. |
| endDateTime | falsum | number | Tempus correspondens in secundis (inclusivum), e.g., 1672387200. |
| orderBy | falsum | string | "createdAt" vel "return", modus ordinationis: "createdAt" (tempus update, default) vel "return" (reditus). |
| orderDirection | falsum | string | "desc" vel "asc", default ad "desc" (ordo descendens). |
| wallet | falsum | string | Inscriptionis loculi (si vacua, omnes inscriptiones loculorum interrogabuntur). |

Nota: Si numerus eventuum 300 excedit intra specificatum `startDateTime` et `endDateTime`, polling adhiberi potest (in hoc casu, `orderBy` debet poni ad `"createdAt"`) ut interrogationem continuet. Parametrus `endDateTime` pro nth polling petitione debet poni ad `createdAt` ultimi recordis ex (n-1)th polling eventu. Postquam omnia data recuperata sunt, duplicata removenda sunt secundum campum `id`.

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Ut infra demonstratum |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string | Product ID correspondens positioni (convenit `productId` in The Graph data). Hic ID unicus est intra dimensionem chain + vault et adhiberi potest ad interrogandum positiones saldos. |
| positionId | string | Position ID, quae unica est intra dimensionem chain. |
| product | object | Informationes de productum. |
| wallet | string | Inscriptionis loculi. |
| createdAt | number | Tempus correspondens in secundis, e.g., 1672387200. |
| updatedAt | number | Tempus correspondens in secundis, e.g., 1672387200. |
| claimed | boolean | Utrum positio redempta sit. |
| takerAllocationRate | number | Secundum tempus calculationis, aestimat proportionem quam dominus ex piscina sponsionum capere potest. Ante exspirationem, haec est aestimatio. |
| triggerTime | number | Pro Rangebound, primum tempus rupturae; pro non-Rangebound, tempus solutionis, in secundis. |
| triggerPrice | number | Pro Rangebound, primum pretium rupturae; pro non-Rangebound, pretium solutionis. |
| feeRate | object | Rate mercedis. |
| leverageInfo | object | Informatiō de mutū (optīōnālis). |
| relevantDollarPrices | list[object] | Pretiī tŏkēnōrum necessāria ad conversionem pretiī RCH et calculum airdrop. |
| amounts | object | Calculātae quantitātēs. |
| apyInfo | object | Informatiō annuālis, praesto pro productīs non-Surge. |
| oddsInfo | object | Informatiō de probabilitātibus, praesto pro productīs Surge. |
| claimParams | object | Informatiō de parametīs reclamandī. |

**Response Example**

```
{
    "value": [{
        ...
    }]
}
```

### Accipere Transactiones Fiscales

```
POST /rfq/transaction-list
```

**Input Parameters**

| **Nomen Campi** | **Requirētur** | **Typus** | **Descriptiō** |
| --- | --- | --- | --- |
| chainId | verum | Int |  |
| vaults | falsum | list[string] | Set contractuum adresses; si non praebentur, omnia contracta interrogabuntur. |
| limit | falsum | Int | Numerus interrogationum; ad 100, maximum est 300. |
| startDateTime | falsum | number | Tempus correspondens in secundis (inclusivum), e.g., 1672387200. |
| endDateTime | falsum | number | Tempus correspondens in secundis (inclusivum), e.g., 1672387200. |
| orderDirection | falsum | string | "desc" aut "asc", ad "desc" (ordo descendens) per default. |
| taker | falsum | string | Adressā fiscā taker |
| maker | falsum | string | Adressā fiscā maker |
| claimParams | falsum | object | Parametra redemptionis, correspondens ad campum claimParams in data position-list. |
| hash | falsum | string | Hash transactionis |

Nota: Parametra taker, maker, et claimParams adhiberi possunt ad coniunctam interrogationem ad inveniendum recorda commercii correspondentia ad positionem (una positio plures commercia correspondere potest) (non omnia parametra requiruntur).
Si numerus resultatium 300 excedit intra specificatum `startDateTime` et `endDateTime`, polling adhiberi potest (in hoc casu, `orderBy` debet poni ad `"createdAt"`) ad continuandum interrogationem. Parametrum `endDateTime` pro nth polling petitione debet poni ad `createdAt` ultimi recordi ex (n-1)th polling result. Postquam omnia data recepta sunt, duplicata removenda sunt secundum campum `id`.

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Ut infra demonstratum est |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | Transactionis hash |
| takerWallet | string | address pecuniae accipientis. |
| makerWallet | string | address pecuniae facientis. |
| product | object | Informationes de producto. |
| createdAt | number | Tempus correspondens in secundis, e.g., 1672387200. |
| takerAllocationRate | number | Secundum tempus calculationis, aestimat proportionem quam dominus ex piscina sponsionum capere potest. Ante expirationem, hoc est aestimatum. |
| triggerTime | number | Pro Rangebound, primum tempus eruptionis; pro non-Rangebound, tempus solutionis, in secundis. |
| triggerPrice | number | Pro Rangebound, primum pretium eruptionis; pro non-Rangebound, pretium solutionis. |
| feeRate | object | Rate feorum. |
| leverageInfo | object | Informationes de mutuo (optativum). |
| relevantDollarPrices | list[object] | Pretium tokenum necessarium pro conversione RCH et calculatione airdrop. |
| amounts | object | Aestimatae quantitates. |
| apyInfo | object | Informationes annualizatae, praesto pro non-Surge productis. |
| oddsInfo | object | Informationes de probabilitatibus, praesto pro Surge productis. |

**Response Example**

```
{
    "value": [{
        ...
    }]
}
```

### Occulta damna positionum

```
POST rfq/position/conceal
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | verum | int | Chain ID |
| positionIds | verum | List[string] | lista positionId，usque ad 20 |

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
| code           | int          | 0 indicat quod resultatum redditum normale est   |
| message        | string       | Nuntius erroris redditus in casu exceptionis |
| value          | object       | .                               |


### Get Pending Transactions

Hoc praecipue adhibetur ad quaestionem synchronisationis datae positionis subiacente. Data hic recuperata foris fieri possunt (propter competitionem bifurcationis blockchain).

```
POST rfq/transactions/pending
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | verum | Int |  |
| vaults | falsum | list[string] | Conjunctio inscriptionum contractuum; si non praebetur, omnes contractus interrogabuntur. |
| taker | falsum | string | Inscriptionem wallet taker |
| maker | falsum | string | Inscriptionem wallet maker |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | numerus |  |
| message | string |  |
| value | list[object] | Ut infra demonstratum |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | Hash transactionis |
| takerWallet | string | inscriptionem wallet taker. |
| makerWallet | string | inscriptionem wallet maker. |
| product | object | Notitia producti. |
| createdAt | numerus | Tempus correspondens in secundis, e.g., 1672387200. |
| takerAllocationRate | numerus | Secundum tempus calculationis, aestimat proportionem quam dominus ex piscina sponsionum capere potest. Ante expirationem, hoc est aestimatum valorem. |
| triggerTime | numerus | Pro Rangebound, primum tempus eruptionis; pro non-Rangebound, tempus solutionis, in secundis. |
| triggerPrice | numerus | Pro Rangebound, primum pretium eruptionis; pro non-Rangebound, pretium solutionis. |
| feeRate | object | Rate mercedis. |
| leverageInfo | object | Notitia mutui (optativa). |
| relevantDollarPrices | list[object] | Pretium tokenorum necessarium ad conversionem pretii RCH et calculationem airdrop. |
| amounts | object | Aestimatae quantitates. |
| apyInfo | object | Notitia annualis, praesto pro productis non-Surge. |
| oddsInfo | object | Notitia de probabilitate, praesto pro productis Surge. |


### Accipe Historiam Airdrop RCH

```
GET rfq/airdrop/history
```

**Input Parameters**

| **Field Name** | **Required** | **Type** | **Description** |
| --- | --- | --- | --- |
| wallet | verum | string | Adresae nummariae |
| startDateTime | verum | number | Tempus correspondens in secundis (inclusivum), e.g., 1672387200. |
| endDateTime | verum | number | Tempus correspondens in secundis (inclusivum), e.g., 1672387200. |
| orderBy | falsum | string | "dateTime" vel "rch", modus ordinandi: "dateTime" (tempus airdrop, default) vel "rch" (quantitas). |
| orderDirection | falsum | string | "desc" vel "asc", default ad "desc" (ordo descendens). |

**Response Parameters**

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Ut infra demonstratum |

Object

| **Field Name** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| dateTime | long | Tempus currente in secundis, exempli gratia, 1672387200. |
| wallet | string | Adresae nummariae |
| volume | string | volumine negotiationis |
| rch | string | Quantitas airdrop, valor raw, dividendum est per 1e18 |
| merkleProof | string | Probatio Merkle  |
