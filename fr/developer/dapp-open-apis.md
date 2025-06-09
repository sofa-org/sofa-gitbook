# DApp Open APIs

Notre dApp existante sur sofa.org est une fonctionnalité critique de flux de travail et est le principal moyen pour les utilisateurs d'interagir avec les protocoles SOFA. Cependant, nous encourageons d'autres développeurs à permettre l'accès et à se connecter à SOFA via leurs propres dApps afin de maximiser la croissance de notre écosystème. Nous considérons ces derniers comme nos partenaires 'Broker' et encourageons les parties intéressées à contacter l'équipe SOFA pour plus d'informations sur l'API.

## DNT

### Demande de liste RFQ DNT recommandée

```
GET /rfq/dnt/recommended-list
```

**Paramètres d'entrée**

| **Nom du champ**       | **Requis**  | **Type** | **Description**                                                                                           |
| ---------------------- | ----------- | -------- | --------------------------------------------------------------------------------------------------------- |
| vault                  | vrai        | string   | Informations sur le contrat                                                                                          |
| chainId                | vrai        | int      | ID de la chaîne                                                                                                   |

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| ---------------- | ------------ | ---------------------------------------------- |
| code             | int          | 0 indique que le résultat retourné est normal   |
| message          | string       | Message d'erreur retourné en cas d'exception |
| value            | list[object] | Comme indiqué ci-dessous                                               |

Objet

| **Nom du champ**               | **Type**     | **Description**                                                                                   |
| ------------------------------ | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                          | number       | ID RFQ                                                                                |
| chainId                        | int          | ID de la chaîne                                                                                      |
| vault                          | string       | Adresse du contrat                                                                                  |
| riskType                       | string       | Type de risque : PROTECTED, RISKY                                                                       |
| forCcy                         | string       | Monnaie sous-jacente                                                                               |
| domCcy                         | string       | Paire de devises                                                                                     |
| depositCcy                     | string       | Monnaie de souscription                                                                             |
| lowerBarrier                   | number       | Prix inférieur                                                                                       |
| upperBarrier                   | number       | Prix supérieur                                                                                       |
| depositAmount                | number       | Montant d'achat RFQ                                                                               |
| expiry                       | number         | Horodatage d'expiration (par exemple, 1672387200)                                                  |
| timestamp                    | number         | Heure de déclenchement du prix actuel ; le prochain temps de début d'observation est calculé sur cette logique |
| observationStart             | number         | Heure de début estimée de l'observation pour le knock in/out basée sur l'horodatage               |
| feeRate                      | object         | Taux de frais de trading et de règlement (optionnel)                                               |
| leverageInfo                 | object         | Informations sur le prêt (optionnel)                                                              |
| relevantDollarPrices         | list[object]   | Prix du token requis pour la conversion de prix RCH et le calcul de l'airdrop (optionnel)         |
| amounts                      | object         | Montants calculés (optionnel)                                                                     |
| apyInfo                     | object         | Informations annualisées, disponibles pour les produits non-Surge (optionnel)                     |
| oddsInfo                     | object         | Informations sur les cotes, disponibles pour les produits Surge (optionnel)                       |
| quote                        | object       |                                                                                                   |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Montant de la garantie du Maker                                                                    |
| \> totalCollateral            | string       | Montant total de la garantie (Taker+Maker)                                                        |
| \> collateralAtRisk          | string       | Requis lorsque garanti (optionnel)                                                                |
| \> makerBalanceThreshold      | string       | Seuil de solde du maker                                                                            |
| \> deadline                   | number         | Horodatage d'expiration (par exemple, 1672387200)                                                |
| \> makerWallet                | string       | Portefeuille du Maker (optionnel)                                                                  |
| \> signature                  | string       | Signature (optionnel)                                                                               |

**Exemple de demande**

```
GET rfq/dnt/recommended-list?vault=xxxxxx&chainId=1
```

**Réponse**

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

### Demande DNT

- Remarques :
  - Veuillez ne pas transmettre l'adresse du portefeuille de l'utilisateur lors d'une demande de simple enquête.
  - L'adresse du portefeuille d'un utilisateur ne doit être transmise qu'après abonnement.

```
GET /rfq/dnt/quote
```

**Paramètres d'entrée**

| **Nom du champ**   | **Requis** | **Type** | **Description**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| coffre              | vrai         | chaîne   | Informations sur le contrat                                                                                                   |
| chainId            | vrai         | int      | ID de la chaîne                                                                                                               |
| expiry             | vrai         | nombre   | Horodatage en secondes pour la date d'expiration, par exemple, 1672387200                                                           |
| lowerBarrier       | vrai         | nombre   | Prix inférieur                                                                                                            |
| upperBarrier       | vrai         | nombre   | Prix supérieur                                                                                                            |
| depositAmount      | vrai         | nombre   | Montant d'achat RFQ                                                                                                    |
| inputApyDefinition | vrai         | chaîne   | Le code sous-jacent est Enum indiquant comment l'APY d'entrée est calculé : OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | faux         | nombre   | Rendement annuel garanti (vide pour RISKY, requis pour protégé)                                                      |
| fundingApy         | faux         | nombre   | Rendement annuel AAVE (vide pour RISKY, requis pour protégé)                                                            |
| takerWallet        | faux         | chaîne   | Informations sur l'adresse publique du portefeuille de l'enquêteur                                                                           |

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indique que le résultat de retour est normal   |
| message        | chaîne       | Message d'erreur retourné en cas d'exception |
| value          | objet        | Comme indiqué ci-dessous                                 |

Objet

| **Nom du champ**               | **Type**     | **Description**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | nombre       | ID RFQ                                                                                |
| coffre                        | chaîne       | Adresse du contrat                                                                                  |
| chainId                      | int          | ID de la chaîne                                                                                          |
| riskType                     | chaîne       | Type de risque : PROTÉGÉ, RISQUÉ                                                                       |
| forCcy                       | chaîne       | Devise sous-jacente                                                                               |
| domCcy                       | chaîne       | Paire de devises                                                                                     |
| depositCcy                   | chaîne       | Devise de souscription                                                                             |
| lowerBarrier                 | nombre       | Prix inférieur                                                                                       |
| upperBarrier                 | nombre       | Prix supérieur                                                                                       |
| depositAmount                | nombre       | Montant d'achat RFQ                                                                               |
| expiry                       | nombre       | Horodatage d'expiration (par exemple, 1672387200)                                                               |
| timestamp                    | nombre       | Heure de déclenchement du prix actuel ; le prochain temps de début d'observation est calculé sur cette logique |
| observationStart             | nombre       | Heure de début estimée de l'observation pour le knock in/out basée sur l'horodatage                        |
| feeRate                      | objet        | Taux de frais de négociation et de règlement (optionnel)                        |
| leverageInfo                 | objet        | Informations sur le prêt (optionnel)                        |
| relevantDollarPrices         | liste[objet] | Prix des tokens requis pour la conversion de prix RCH et le calcul des airdrops (optionnel)                       |
| amounts                      | objet        | Montants calculés (optionnel)                       |
| apyInfo             | objet         | Informations annualisées, disponibles pour les produits non-Surge (optionnel)                     |
| oddsInfo             | objet         | Informations sur les cotes, disponibles pour les produits Surge (optionnel)                    |
| quote                        | objet       |                                                                                                   |
| \> quoteId               | nombre |                                                                           |
| \> anchorPrices               | liste[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | chaîne       | Montant de la garantie du Maker                                                                         |
| \> totalCollateral            | chaîne       | Montant total de la garantie (Taker+Maker)                                                             |
| \> collateralAtRisk | chaîne       | Requis lorsque garanti (optionnel)                                                           |
| \> makerBalanceThreshold | chaîne       | seuil de solde du maker                                                          |
| \> deadline                   | nombre         | Horodatage d'expiration (par exemple, 1672387200)                                                           |
| \> makerWallet                | chaîne       | Portefeuille du Maker (optionnel)                                                                     |
| \> signature                  | chaîne       | Signature (optionnel)                                                                          |

### Probabilité de Gain DNT

Probabilité de rester dans les limites jusqu'à l'expiration

```
GET rfq/dnt/winning-probabilities
```

**Paramètres d'Entrée**

| **Nom du Champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| forCcy          | vrai         | chaîne       | Monnaie sous-jacente      |
| expiry          | vrai         | nombre     | Horodatage de second niveau pour la date d'expiration, par exemple, 1672387200 |
| lowerBarrier       | vrai         | nombre   | Prix inférieur |
| upperBarrier       | vrai         | nombre   | Prix supérieur |

**Paramètres de Réponse**

| **Nom du Champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indique que le résultat retourné est normal   |
| message        | chaîne       | Message d'erreur retourné en cas d'exception |
| value          | liste[objet] | Comme indiqué ci-dessous                                 |

Objet

| **Nom du champ** | **Type**     | **Description**                                |
| ---------------- | ------------ | ---------------------------------------------- |
| spotPrice        | number       | Prix au comptant                               |
| timestamp        | number       |                                                |
| probabilities    | object       | Probabilité de gain                            |

**Exemple de demande**

```
GET rfq/dnt/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerBarrier=xxx&upperBarrier=xxx
```

**Réponse**

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

## Tendance Intelligente

### Demande de liste RFQ de Tendance Intelligente recommandée

```
GET rfq/smart-trend/recommended-list
```

**Paramètres d'entrée**

| **Nom du champ**       | **Requis**  | **Type** | **Description**                                         |
| ---------------------- | ----------- | -------- | ------------------------------------------------------ |
| vault                  | vrai        | chaîne   | Informations sur le contrat                            |
| chainId                | vrai        | int      | ID de la chaîne                                       |

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| ---------------- | ------------ | ---------------------------------------------- |
| code             | int          | 0 indique que le résultat retourné est normal  |
| message          | chaîne       | Message d'erreur retourné en cas d'exception   |
| value            | liste[objet] | Comme indiqué ci-dessous                        |

Objet

| **Nom du champ**               | **Type**     | **Description**                                                                                   |
| ------------------------------ | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                          | nombre       | ID RFQ                                                                                           |
| chainId                        | int          | ID de la chaîne                                                                                  |
| vault                          | chaîne       | Adresse du contrat                                                                                |
| riskType                       | chaîne       | Type de risque : PROTÉGÉ, RISQUÉ                                                                 |
| direction                      | chaîne       | BAISSIER, HAUSSER                                                                                 |
| forCcy                         | chaîne       | Devise sous-jacente                                                                               |
| domCcy                         | chaîne       | Paire de devises                                                                                 |
| depositCcy                     | chaîne       | Devise de souscription                                                                            |
| lowerBarrier                   | nombre       | Prix inférieur                                                                                    |
| upperBarrier                   | nombre       | Prix supérieur                                                                                    |
| depositAmount                  | nombre       | Montant d'achat RFQ                                                                               |
| expiry                         | nombre       | Horodatage d'expiration (par exemple, 1672387200)                                               |
| timestamp                      | nombre       | Heure de déclenchement du prix actuel ; le prochain temps de début d'observation est calculé sur cette logique |
| feeRate                        | objet        | Taux de frais de trading et de règlement (facultatif)                                            |
| leverageInfo                   | objet        | Informations sur le prêt (facultatif)                                                            |
| relevantDollarPrices           | liste[objet] | Prix des tokens requis pour la conversion de prix RCH et le calcul des airdrops (facultatif)   |
| amounts                        | objet        | Montants calculés (facultatif)                                                                   |
| apyInfo                        | objet        | Informations annualisées, disponibles pour les produits non-Surge (facultatif)                  |
| oddsInfo                       | objet        | Informations sur les cotes, disponibles pour les produits Surge (facultatif)                    |
| quote                          | objet        |                                                                                                   |
| \> anchorPrices                | liste[chaîne] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Montant de la garantie du Maker                                                                         |
| \> totalCollateral            | string       | Montant total de la garantie (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Requis lorsque garanti (optionnel)                                                           |
| \> makerBalanceThreshold | string       | Seuil de solde du maker                                                          |
| \> deadline                   | number         | Horodatage d'expiration (par exemple, 1672387200)                                                           |
| \> makerWallet                | string       | Portefeuille du Maker (optionnel)                                                                     |
| \> signature                  | string       | Signature (optionnel)                                                                          |

**Exemple de demande**

```
GET rfq/smart-trend/recommended-list?vault=xxxxxx
```

### Enquête sur la tendance intelligente

- Remarques :
  - Veuillez ne pas transmettre l'adresse du portefeuille de l'utilisateur lors d'une demande d'enquête pure.
  - L'adresse du portefeuille d'un utilisateur ne doit être transmise qu'après abonnement.

```
GET /rfq/smart-trend/quote
```

**Paramètres d'entrée**

| **Nom du champ**     | **Requis** | **Type** | **Description**                                                                                                        |
| ------------------ | ------------ | -------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | Informations sur le contrat                                                                                                   |
| chainId            | true         | int      | ID de la chaîne                                                                                                               |
| expiry             | true         | number     | Horodatage en secondes pour la date d'expiration, par exemple, 1672387200                                                           |
| lowerBarrier       | true         | number   | Prix inférieur                                                                                                            |
| upperBarrier       | true         | number   | Prix supérieur                                                                                                            |
| depositAmount      | true         | number   | Montant d'achat RFQ                                                                                                    |
| inputApyDefinition | true         | string   | Le code sous-jacent est Enum indiquant comment le rendement annuel en pourcentage (APY) est calculé : OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | Rendement annuel garanti (vide pour RISKY, requis pour protégé)                                                      |
| fundingApy         | false        | number   | Rendement annuel AAVE (vide pour RISKY, requis pour protégé)                                                            |
| takerWallet        | false        | string   | Informations sur l'adresse publique du portefeuille de l'enquêteur                                                                           |

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| ---------------- | ------------ | ---------------------------------------------- |
| code             | int          | 0 indique que le résultat de retour est normal   |
| message          | string       | Message d'erreur renvoyé en cas d'exception |
| value            | object       | Comme indiqué ci-dessous                                 |

Objet

| **Nom du champ**               | **Type**     | **Description**                                                                                   |
| ------------------------------ | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                          | number       | ID de la RFQ                                                                                |
| vault                          | string       | Adresse du contrat                                                                                  |
| chainId                        | int          | ID de la chaîne                                                                                          |
| riskType                       | string       | Type de risque : PROTÉGÉ, RISQUÉ                                                                       |
| direction                      | string       | HAUSSIER,BAISSIER | 
| forCcy                         | string       | Devise sous-jacente                                                                               |
| domCcy                         | string       | Paire de devises                                                                                     |
| depositCcy                     | string       | Devise d'abonnement                                                                             |
| lowerBarrier                   | number       | Prix inférieur                                                                                       |
| upperBarrier                   | number       | Prix supérieur                                                                                       |
| depositAmount                  | number       | Montant d'achat de la RFQ                                                                               |
| expiry                         | number       | Horodatage d'expiration (par exemple, 1672387200)                                                               |
| timestamp                      | number       | Heure de déclenchement du prix actuel ; le prochain temps de début d'observation est calculé sur cette logique |
| feeRate                        | object       | Taux de frais de trading et de règlement (optionnel)                        |
| leverageInfo                   | object       | Informations sur le prêt (optionnel)                        |
| relevantDollarPrices           | list[object] | Prix des jetons requis pour la conversion de prix RCH et le calcul des airdrops (optionnel)                       |
| amounts                        | object       | Montants calculés (optionnel)                       |
| apyInfo                        | object       | Informations annualisées, disponibles pour les produits non-Surge (optionnel)                     |
| oddsInfo                       | object       | Informations sur les cotes, disponibles pour les produits Surge (optionnel)                    |
| quote                          | object       |                                                                                                   |
| \> quoteId                    | number |                                                                           |
| \> anchorPrices                | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral             | string       | Montant de la garantie du maker                                                                         |
| \> totalCollateral             | string       | Montant total de la garantie (Taker+Maker)                                                             |
| \> collateralAtRisk            | string       | Requis lorsque garanti (optionnel)                                                           |
| \> makerBalanceThreshold       | string       | Seuil de solde du maker                                                          |
| \> deadline                    | number       | Horodatage d'expiration (par exemple, 1672387200)                                                           |
| \> makerWallet                 | string       | Portefeuille du maker (optionnel)                                                                     |
| \> signature                   | string       | Signature (optionnel)                                                                          |

### Probabilité de Gain de Smart Trend

Probabilité de rester dans les limites jusqu'à l'expiration

```
GET rfq/smart-trend/winning-probabilities
```

**Paramètres d'Entrée**

| **Nom du Champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| forCcy          | vrai         | chaîne       | Monnaie sous-jacente      |
| expiry          | vrai         | nombre     | Horodatage de deuxième niveau pour la date d'expiration, par exemple, 1672387200 |
| lowerBarrier       | vrai         | nombre   | Prix inférieur |
| upperBarrier       | vrai         | nombre   | Prix supérieur |

**Paramètres de Réponse**

| **Nom du Champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indique que le résultat retourné est normal   |
| message        | chaîne       | Message d'erreur retourné en cas d'exception |
| value          | liste[objet] | Comme indiqué ci-dessous                                 |

Objet

| **Nom du Champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| spotPrice | nombre | Prix au comptant |
| timestamp | nombre |  |
| probabilities | objet | Probabilité de gain  |


**Exemple de Demande**

```
GET rfq/smart-trend/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerStrike=xxx&upperStrike=xxx
```

**Réponse**

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

## Interface Générique

### Supprimer RFQ

Supprimez le RFQ qui a été enregistré sur la chaîne

```
POST rfq/remove
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| rfqId          | vrai         | nombre       | .       |

**Exemple de demande**

```
{  
"rfqId":123456  
}
```

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indique que le résultat de retour est normal   |
| message        | string       | Message d'erreur retourné en cas d'exception |
| value          | object       | .                               |


### Notification de transaction

```
POST rfq/trade
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| quotes | vrai | list[object] |   |
| \> rfqId | vrai | number |     |
| \> quoteId | vrai | number |     |
| \> txId | vrai | string | Hachage de la transaction |
| code | faux | string | Code d'invitation |
| walletType | faux | string | Type de portefeuille, tel que MetaMask, OKX Wallet, Coinbase, etc. |

**Exemple de demande**

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

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indique que le résultat de retour est normal   |
| message        | string       | Message d'erreur retourné en cas d'exception   |
| value          | object       | .                               |

### Liste des strikes

```
GET rfq/strike-list
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| indexPrice | vrai | number |  |
| forCcy | faux | string | Pour la paire de trading BTC-USDT, entrez BTC ici ; si non fourni, la configuration par défaut sera utilisée. |
| domCcy | faux | string | Pour la paire de trading BTC-USDT, entrez USDT ici ; si non fourni, cela par défaut à USDT. |


**Exemple de demande**

```
{  
"indexPrice": 3750.8，  
"forCcy": "WBTC",  
}
```

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| ---------------- | ------------ | ---------------------------------------------- |
| strikes          | list[number] | Liste de prix d'exercice recommandée par défaut |

### Liste des expirations

```
GET rfq/expiry-list
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| ---------------- | ---------- | -------- | ---------------- |
| vault            | vrai       | string   | Adresse du contrat |
| chainId         | vrai       | int      | ID de la chaîne |


**Exemple de requête**

```
{  
"vault": "XXXXXXXXXXXXXX",  
"chainId": 1,  
}
```

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| ---------------- | ------------ | ---------------------------------------------- |
| timestamp        | number       | Heure de début de référence en secondes timestamp |
| expiries         | list[number] | Liste des expirations prises en charge en secondes timestamp, par exemple, 1672387200 |

### Aave apy

```
GET rfq/aave-apy
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | vrai | nombre |     |
| ccy | vrai | chaîne | USDT |

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | nombre | ID de la chaîne |
| ccy | chaîne | Devise de dépôt |
| avgApy | chaîne | APY moyen enregistré sur la chaîne au cours des 30 derniers jours |
| currentApy | chaîne | Dernier APY AAVE |
| apyUsed | chaîne | L'APY réellement utilisé dans SofaServer pour estimer les revenus d'intérêts futurs |
| apyDefinition | chaîne | Définition de calcul correspondant à l'APY |

**Exemple de réponse**

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

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | vrai | nombre |     |
| ccy | vrai | chaîne | USDT |
| apyDefinition | faux | chaîne | Définition du calcul correspondant à l'APY ; par défaut AaveLendingApy |


**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | nombre | ID de la chaîne |
| ccy | chaîne | Devise de dépôt |
| avgApy | chaîne | APY moyen enregistré sur la chaîne au cours des 30 derniers jours |
| currentApy | chaîne | Dernier APY |
| apyUsed | chaîne | L'APY effectivement utilisé dans SofaServer pour estimer les revenus d'intérêts futurs |
| apyDefinition | chaîne | Définition du calcul correspondant à l'APY |

**Exemple de réponse**

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

### Obtenir les positions de portefeuille

```
POST /rfq/position-list
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |

| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | Ensemble d'adresses de contrats ; si non fourni, tous les contrats seront interrogés. |
| claimed | false | boolean | Indique s'il a été échangé ; si non fourni, les positions de tous les statuts seront interrogées. |
| expired | false | boolean | Indique s'il a expiré ; si non fourni, les positions de tous les statuts seront interrogées. |
| concealed | false | boolean | Indique s'il est caché ; si non fourni, les positions de tous les statuts seront interrogées. |
| positiveReturn | false | boolean | Indique si le montant de rachat est supérieur à 0 ; si non fourni, toutes les positions seront interrogées. |
| positiveProfit | false | boolean | Indique si le retour dépasse le principal ; si non fourni, toutes les positions seront interrogées. |
| limit | false | Int | Nombre de requêtes ; par défaut 100, maximum 300. |
| startDateTime | false | number | Horodatage correspondant en secondes (inclus), par exemple, 1672387200. |
| endDateTime | false | number | Horodatage correspondant en secondes (inclus), par exemple, 1672387200. |
| orderBy | false | string | "createdAt" ou "return", méthode de tri : "createdAt" (heure de mise à jour, par défaut) ou "return" (retours). |
| orderDirection | false | string | "desc" ou "asc", par défaut "desc" (ordre décroissant). |
| wallet | false | string | Adresse de portefeuille (si vide, toutes les adresses de portefeuille sont interrogées). |

Note : Si le nombre de résultats dépasse 300 dans la plage de `startDateTime` et `endDateTime` spécifiée, le sondage peut être utilisé (dans ce cas, `orderBy` doit être défini sur `"createdAt"`) pour continuer à interroger. Le paramètre `endDateTime` pour la n-ième requête de sondage doit être défini sur le `createdAt` du dernier enregistrement du résultat de sondage (n-1). Après avoir récupéré toutes les données, les doublons doivent être supprimés en fonction du champ `id`.

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Comme indiqué ci-dessous |

Objet

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string | ID du produit correspondant à la position (correspond à `productId` dans les données de The Graph). Cet ID est unique dans la dimension chaîne + coffre et peut être utilisé pour interroger les soldes de position. |
| positionId | string | ID de la position, qui est unique dans la dimension chaîne. |
| product | object | Informations sur le produit. |
| wallet | string | Adresse de portefeuille. |
| createdAt | number | Horodatage correspondant en secondes, par exemple, 1672387200. |
| updatedAt | number | Horodatage correspondant en secondes, par exemple, 1672387200. |
| claimed | boolean | Indique si la position a été échangée. |
| takerAllocationRate | number | Basé sur le temps de calcul, estime la proportion que le propriétaire peut prendre du pool de paris. Avant l'expiration, il s'agit d'une valeur estimée. |
| triggerTime | number | Pour Rangebound, le premier temps de rupture ; pour non-Rangebound, le temps de règlement, en secondes. |
| triggerPrice | number | Pour Rangebound, le premier prix de rupture ; pour non-Rangebound, le prix de règlement. |
| feeRate | object | Taux de frais. |
| leverageInfo | objet | Informations sur le prêt (facultatif). |
| relevantDollarPrices | liste[objet] | Prix des tokens requis pour la conversion de prix RCH et le calcul de l'airdrop. |
| amounts | objet | Montants calculés. |
| apyInfo | objet | Informations annualisées, disponibles pour les produits non-Surge. |
| oddsInfo | objet | Informations sur les cotes, disponibles pour les produits Surge. |
| claimParams | objet | Informations sur les paramètres de réclamation. |

**Exemple de réponse**

```
{
    "value": [{
        ...
    }]
}
```

### Obtenir les transactions de portefeuille

```
POST /rfq/transaction-list
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | vrai | Int |  |
| vaults | faux | liste[string] | Ensemble d'adresses de contrat ; si non fourni, tous les contrats seront interrogés. |
| limit | faux | Int | Nombre de requêtes ; par défaut 100, maximum 300. |
| startDateTime | faux | nombre | Horodatage correspondant en secondes (inclus), par exemple, 1672387200. |
| endDateTime | faux | nombre | Horodatage correspondant en secondes (inclus), par exemple, 1672387200. |
| orderDirection | faux | chaîne | "desc" ou "asc", par défaut "desc" (ordre décroissant). |
| taker | faux | chaîne | Adresse du portefeuille du preneur |
| maker | faux | chaîne | Adresse du portefeuille du créateur |
| claimParams | faux | objet | Les paramètres de rachat, correspondant au champ claimParams dans les données de la liste de positions. |
| hash | faux | chaîne | Hash de la transaction |

Remarque : Les paramètres taker, maker et claimParams peuvent être utilisés pour une requête conjointe afin de trouver les enregistrements de trade correspondants à une position (une position peut correspondre à plusieurs trades) (tous les paramètres ne sont pas requis). Si le nombre de résultats dépasse 300 dans la plage de `startDateTime` et `endDateTime` spécifiée, un sondage peut être utilisé (dans ce cas, `orderBy` doit être défini sur `"createdAt"`) pour continuer à interroger. Le paramètre `endDateTime` pour la n-ième requête de sondage doit être défini sur le `createdAt` du dernier enregistrement du résultat de sondage (n-1). Après avoir récupéré toutes les données, les doublons doivent être supprimés en fonction du champ `id`.

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Comme indiqué ci-dessous |

Objet

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | Hash de la transaction |
| takerWallet | string | Adresse du portefeuille du preneur. |
| makerWallet | string | Adresse du portefeuille du créateur. |
| product | object | Informations sur le produit. |
| createdAt | number | Horodatage correspondant en secondes, par exemple, 1672387200. |
| takerAllocationRate | number | En fonction du temps de calcul, estime la proportion que le propriétaire peut prendre du pool de paris. Avant l'expiration, il s'agit d'une valeur estimée. |
| triggerTime | number | Pour Rangebound, le premier moment de rupture ; pour non-Rangebound, le moment de règlement, en secondes. |
| triggerPrice | number | Pour Rangebound, le premier prix de rupture ; pour non-Rangebound, le prix de règlement. |
| feeRate | object | Taux de frais. |
| leverageInfo | object | Informations sur le prêt (optionnel). |
| relevantDollarPrices | list[object] | Prix des tokens nécessaires pour la conversion de prix RCH et le calcul des airdrops. |
| amounts | object | Montants calculés. |
| apyInfo | object | Informations annualisées, disponibles pour les produits non-Surge. |
| oddsInfo | object | Informations sur les cotes, disponibles pour les produits Surge. |

**Exemple de réponse**

```
{
    "value": [{
        ...
    }]
}
```

### Masquer les positions de perte

```
POST rfq/position/conceal
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | vrai | int | ID de la chaîne |
| positionIds | vrai | List[string] | liste des positionId, jusqu'à 20 |

**Exemple de requête**

```
{
    "positionIds": [
        "aaaa","bbbb"
    ],
    "chainId": 1
}
```

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 indique que le résultat retourné est normal   |
| message        | string       | Message d'erreur retourné en cas d'exception  |
| value          | object       | .                               |


### Obtenir les transactions en attente

Ceci vise principalement à résoudre le problème de synchronisation des données de position sous-jacentes. Les données récupérées ici peuvent disparaître (en raison de la concurrence des forks de blockchain).

```
POST rfq/transactions/pending
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| chainId | vrai | Int |  |
| vaults | faux | list[string] | Ensemble d'adresses de contrats ; si non fourni, tous les contrats seront interrogés. |
| taker | faux | string | Adresse du portefeuille du preneur |
| maker | faux | string | Adresse du portefeuille du créateur |

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Comme indiqué ci-dessous |

Objet

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | Hash de la transaction |
| takerWallet | string | adresse du portefeuille du preneur. |
| makerWallet | string | adresse du portefeuille du créateur. |
| product | object | Informations sur le produit. |
| createdAt | number | Horodatage correspondant en secondes, par exemple, 1672387200. |
| takerAllocationRate | number | En fonction du temps de calcul, estime la proportion que le propriétaire peut prendre du pool de paris. Avant l'expiration, il s'agit d'une valeur estimée. |
| triggerTime | number | Pour Rangebound, le premier temps de rupture ; pour non-Rangebound, le temps de règlement, en secondes. |
| triggerPrice | number | Pour Rangebound, le premier prix de rupture ; pour non-Rangebound, le prix de règlement. |
| feeRate | object | Taux de frais. |
| leverageInfo | object | Informations sur le prêt (facultatif). |
| relevantDollarPrices | list[object] | Prix des tokens nécessaires pour la conversion de prix RCH et le calcul des airdrops. |
| amounts | object | Montants calculés. |
| apyInfo | object | Informations annualisées, disponibles pour les produits non-Surge. |
| oddsInfo | object | Informations sur les cotes, disponibles pour les produits Surge. |


### Obtenir l'historique des airdrops RCH

```
GET rfq/airdrop/history
```

**Paramètres d'entrée**

| **Nom du champ** | **Requis** | **Type** | **Description** |
| --- | --- | --- | --- |
| wallet | vrai | chaîne | Adresse du portefeuille |
| startDateTime | vrai | nombre | Horodatage correspondant en secondes (inclus), par exemple, 1672387200. |
| endDateTime | vrai | nombre | Horodatage correspondant en secondes (inclus), par exemple, 1672387200. |
| orderBy | faux | chaîne | "dateTime" ou "rch", méthode de tri : "dateTime" (temps d'airdrop, par défaut) ou "rch" (montant). |
| orderDirection | faux | chaîne | "desc" ou "asc", par défaut "desc" (ordre décroissant). |

**Paramètres de réponse**

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | nombre |  |
| message | chaîne |  |
| value | liste[objet] | Comme indiqué ci-dessous |

Objet

| **Nom du champ** | **Type**     | **Description**                                |
| -------------- | ------------ | ---------------------------------------------- |
| dateTime | long | L'horodatage actuel en secondes, par exemple, 1672387200. |
| wallet | chaîne | Adresse du portefeuille |
| volume | chaîne | volume de trading |
| rch | chaîne | Montant de l'airdrop, la valeur brute, doit être divisée par 1e18 |
| merkleProof | chaîne | Preuve de Merkle  |
