# DApp Open APIs

Unsere bestehende dApp auf sofa.org ist ein kritisches Workflow-Feature und der Hauptweg für Benutzer, um mit den SOFA-Protokollen zu interagieren. Wir ermutigen jedoch andere Entwickler, den Zugang zu ermöglichen und sich über ihre eigenen dApps mit SOFA zu verbinden, um das Wachstum unseres Ökosystems zu maximieren. Wir betrachten diese als unsere 'Broker'-Partner und ermutigen interessierte Parteien, das SOFA-Team für weitere API-Informationen zu kontaktieren.

## DNT

### Empfohlene DNT RFQ-Liste Anfrage

```
GET /rfq/dnt/recommended-list
```

**Eingabeparameter**

| **Feldname**       | **Erforderlich**  | **Typ** | **Beschreibung**                                                                                           |
| -------------------| ------------- | -------- | ----------------------------------------------------------------------------------------------------------- |
| vault              | true          | string   | Vertragsinformationen                                                                                                   |
| chainId            | true          | int      | Chain-ID                                                                                                               |

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------| ------------ | ---------------------------------------------- |
| code         | int          | 0 zeigt an, dass das Rückgabeergebnis normal ist   |
| message      | string       | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value        | list[object] | Wie unten gezeigt                                               |

Objekt

| **Feldname**               | **Typ**     | **Beschreibung**                                                                                   |
| ---------------------------| ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                      | number       | RFQ-ID                                                                                |
| chainId                    | int          | Chain-ID                                                                                          |
| vault                      | string       | Vertragsadresse                                                                                  |
| riskType                   | string       | Risikotyp: PROTECTED, RISKY                                                                       |
| forCcy                     | string       | Basiswährung                                                                               |
| domCcy                     | string       | Währungspaar                                                                                     |
| depositCcy                 | string       | Zeichnungswährung                                                                             |
| lowerBarrier               | number       | Untere Preis                                                                                       |
| upperBarrier               | number       | Obere Preis                                                                                       |
| depositAmount                | number       | RFQ-Kaufbetrag                                                                                   |
| expiry                       | number         | Ablaufzeitstempel (z. B. 1672387200)                                                             |
| timestamp                    | number         | Auslösezeitpunkt der aktuellen Preisgestaltung; die nächste Beobachtungsstartzeit wird basierend auf dieser Logik berechnet |
| observationStart             | number         | Geschätzte Startzeit der Beobachtung für Knock-In/Out basierend auf dem Zeitstempel              |
| feeRate                      | object         | Handels- und Abwicklungsgebührensatz (optional)                                                  |
| leverageInfo                 | object         | Kreditinformationen (optional)                                                                    |
| relevantDollarPrices         | list[object]   | Tokenpreis, der für die RCH-Preisumrechnung und Airdrop-Berechnung erforderlich ist (optional)   |
| amounts                      | object         | Berechnete Beträge (optional)                                                                     |
| apyInfo                     | object         | Annualisierte Informationen, verfügbar für Nicht-Surge-Produkte (optional)                       |
| oddsInfo                     | object         | Quoteninformationen, verfügbar für Surge-Produkte (optional)                                     |
| quote                        | object       |                                                                                                   |
| \> anchorPrices              | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral           | string       | Sicherheiten des Makers                                                                           |
| \> totalCollateral           | string       | Gesamte Sicherheiten (Taker+Maker)                                                                |
| \> collateralAtRisk          | string       | Erforderlich, wenn garantiert (optional)                                                          |
| \> makerBalanceThreshold      | string       | Maker-Balance-Schwelle                                                                            |
| \> deadline                   | number         | Ablaufzeitstempel (z. B. 1672387200)                                                             |
| \> makerWallet                | string       | Wallet des Makers (optional)                                                                      |
| \> signature                  | string       | Unterschrift (optional)                                                                           |

**Anforderungsbeispiel**

```
GET rfq/dnt/recommended-list?vault=xxxxxx&chainId=1
```

**Antwort**

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
```

```json
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

### DNT Anfrage

- Hinweise:
  - Bitte geben Sie die Wallet-Adresse des Benutzers bei einer reinen Anfrage nicht weiter.
  - Die Wallet-Adresse eines Benutzers sollte nur bei einer Anmeldung weitergegeben werden.

```
GET /rfq/dnt/quote
```

**Eingabeparameter**

| **Feldname**       | **Erforderlich** | **Typ**   | **Beschreibung**                                                                                                        |
| ------------------ | ---------------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true         | string   | Vertragsinformationen                                                                                                   |
| chainId            | true         | int      | Chain-ID                                                                                                               |
| expiry             | true         | number   | Zeitstempel der zweiten Ebene für das Ablaufdatum, z.B. 1672387200                                                           |
| lowerBarrier       | true         | number   | Untere Preisgrenze                                                                                                            |
| upperBarrier       | true         | number   | Obere Preisgrenze                                                                                                            |
| depositAmount      | true         | number   | RFQ-Kaufbetrag                                                                                                    |
| inputApyDefinition | true         | string   | Der zugrunde liegende Code ist Enum, das angibt, wie der Eingabe-APY berechnet wird: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false        | number   | Garantierte jährliche Rendite (leer für RISKY, erforderlich für geschützt)                                                      |
| fundingApy         | false        | number   | AAVE-Jahresrendite (leer für RISKY, erforderlich für geschützt)                                                            |
| takerWallet        | false        | string   | Öffentliche Adressinformationen der Brieftasche des Anfragenden                                                                           |

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 zeigt an, dass das Rückgabeergebnis normal ist   |
| message        | string       | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value          | object       | Wie unten gezeigt                                 |

Objekt

| **Feldname**               | **Typ**     | **Beschreibung**                                                                                   |
| ---------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                        | number       | RFQ-ID                                                                                |
| vault                        | string       | Vertragsadresse                                                                                  |
| chainId                      | int          | Chain-ID                                                                                          |
| riskType                     | string       | Risikotyp: GESCHÜTZT, RISKANT                                                                       |
| forCcy                       | string       | Unterliegende Währung                                                                               |
| domCcy                       | string       | Währungspaar                                                                                     |
| depositCcy                   | string       | Abonnierungswährung                                                                             |
| lowerBarrier                 | number       | Untere Preisgrenze                                                                                       |
| upperBarrier                 | number       | Obere Preisgrenze                                                                                       |
| depositAmount                | number       | RFQ-Kaufbetrag                                                                               |
| expiry                       | number         | Ablaufzeitstempel (z.B. 1672387200)                                                               |
| timestamp                    | number         | Auslösezeitpunkt der aktuellen Preisgestaltung; die nächste Beobachtungsstartzeit wird basierend auf dieser Logik berechnet |
| observationStart             | number         | Geschätzter Startzeitpunkt der Beobachtung für das Ein- und Ausknocken basierend auf dem Zeitstempel                        |
| feeRate                      | object         | Handels- und Abwicklungsgebührensatz (optional)                        |
| leverageInfo                 | object         | Kreditinformationen (optional)                        |
| relevantDollarPrices         | list[object]   | Tokenpreis, der für die RCH-Preiskonvertierung und Airdrop-Berechnung erforderlich ist (optional)                       |
| amounts                      | object         | Berechnete Beträge (optional)                       |
| apyInfo             | object         | Jährliche Informationen, verfügbar für Nicht-Surge-Produkte (optional)                     |
| oddsInfo             | object         | Quoteninformationen, verfügbar für Surge-Produkte (optional)                    |
| quote                        | object       |                                                                                                   |
| \> quoteId               | number |                                                                           |
| \> anchorPrices               | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Betrag der Sicherheiten des Makers                                                                         |
| \> totalCollateral            | string       | Gesamter Sicherheitenbetrag (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Erforderlich, wenn garantiert (optional)                                                           |
| \> makerBalanceThreshold | string       | Maker-Balance-Schwelle                                                          |
| \> deadline                   | number         | Ablaufzeitstempel (z.B. 1672387200)                                                           |
| \> makerWallet                | string       | Wallet des Makers (optional)                                                                     |
| \> signature                  | string       | Unterschrift (optional)                                                                          |

### DNT Gewinnwahrscheinlichkeit

Wahrscheinlichkeit, innerhalb der Grenzen bis zum Ablauf zu bleiben

```
GET rfq/dnt/winning-probabilities
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| forCcy          | true         | string       | Basiswährung      |
| expiry          | true         | number     | Zeitstempel der zweiten Ebene für das Ablaufdatum, z.B. 1672387200 |
| lowerBarrier       | true         | number   | Untere Preisgrenze |
| upperBarrier       | true         | number   | Obere Preisgrenze |

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 zeigt an, dass das Rückgabeergebnis normal ist   |
| message        | string       | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value          | list[object] | Wie unten gezeigt                                 |

Objekt

| **Feldname**      | **Typ**     | **Beschreibung**                                |
| ------------------ | ------------ | ---------------------------------------------- |
| spotPrice          | number      | Spotpreis                                      |
| timestamp          | number      |                                                |
| probabilities      | object      | Gewinnwahrscheinlichkeit                        |

**Anforderungsbeispiel**

```
GET rfq/dnt/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerBarrier=xxx&upperBarrier=xxx
```

**Antwort**

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

## Smart Trend

### Empfohlene Smart Trend RFQ-Liste Anfrage

```
GET rfq/smart-trend/recommended-list
```

**Eingabeparameter**

| **Feldname**       | **Erforderlich**  | **Typ** | **Beschreibung**                                         |
| ------------------ | ----------------- | -------- | -------------------------------------------------------- |
| vault              | wahr              | string   | Vertragsinformationen                                     |
| chainId            | wahr              | int      | Chain-ID                                                |

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| ------------ | ------------ | ---------------------------------------------- |
| code         | int          | 0 zeigt an, dass das Rückgabeergebnis normal ist   |
| message      | string       | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value        | list[object] | Wie unten gezeigt                                 |

Objekt

| **Feldname**               | **Typ**     | **Beschreibung**                                                                                   |
| -------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                      | number       | RFQ-ID                                                                                             |
| chainId                    | int          | Chain-ID                                                                                          |
| vault                      | string       | Vertragsadresse                                                                                    |
| riskType                   | string       | Risikotyp: PROTECTED, RISKY                                                                        |
| direction                  | string       | BULLISH, BEARISH                                                                                  |
| forCcy                     | string       | Basiswährung                                                                                       |
| domCcy                     | string       | Währungspaar                                                                                       |
| depositCcy                 | string       | Zeichnungswährung                                                                                 |
| lowerBarrier               | number       | Untere Preis                                                                                        |
| upperBarrier               | number       | Obere Preis                                                                                        |
| depositAmount              | number       | RFQ-Kaufbetrag                                                                                     |
| expiry                     | number       | Ablaufzeitstempel (z.B. 1672387200)                                                               |
| timestamp                  | number       | Auslösezeitpunkt der aktuellen Preisgestaltung; die nächste Beobachtungsstartzeit wird basierend auf dieser Logik berechnet |
| feeRate                    | object       | Handels- und Abwicklungsgebührensatz (optional)                                                  |
| leverageInfo               | object       | Kreditinformationen (optional)                                                                     |
| relevantDollarPrices       | list[object] | Tokenpreis, der für die RCH-Preisumrechnung und Airdrop-Berechnung erforderlich ist (optional)   |
| amounts                    | object       | Berechnete Beträge (optional)                                                                      |
| apyInfo                   | object       | Annualisierte Informationen, verfügbar für Nicht-Surge-Produkte (optional)                       |
| oddsInfo                   | object       | Quoteninformationen, verfügbar für Surge-Produkte (optional)                                     |
| quote                      | object       |                                                                                                   |
| \> anchorPrices            | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral            | string       | Betrag der Sicherheiten des Makers                                                                         |
| \> totalCollateral            | string       | Gesamter Sicherheitenbetrag (Taker+Maker)                                                             |
| \> collateralAtRisk | string       | Erforderlich, wenn garantiert (optional)                                                           |
| \> makerBalanceThreshold | string       | Schwelle für das Maker-Guthaben                                                          |
| \> deadline                   | number         | Ablaufzeitstempel (z. B. 1672387200)                                                           |
| \> makerWallet                | string       | Wallet des Makers (optional)                                                                     |
| \> signature                  | string       | Unterschrift (optional)                                                                          |

**Anfragebeispiel**

```
GET rfq/smart-trend/recommended-list?vault=xxxxxx
```

### Smart Trend Anfrage

- Hinweise:
  - Bitte geben Sie die Wallet-Adresse des Benutzers bei einer reinen Anfrage nicht weiter.
  - Die Wallet-Adresse eines Benutzers sollte nur bei einer Anmeldung übermittelt werden.

```
GET /rfq/smart-trend/quote
```

**Eingabeparameter**

| **Feldname**       | **Erforderlich** | **Typ**   | **Beschreibung**                                                                                                        |
| ------------------ | ---------------- | --------- | ---------------------------------------------------------------------------------------------------------------------- |
| vault              | true             | string    | Vertragsinformationen                                                                                                   |
| chainId            | true             | int       | Chain-ID                                                                                                               |
| expiry             | true             | number    | Zeitstempel in Sekunden für das Ablaufdatum, z. B. 1672387200                                                           |
| lowerBarrier       | true             | number    | Untere Preisgrenze                                                                                                     |
| upperBarrier       | true             | number    | Obere Preisgrenze                                                                                                     |
| depositAmount      | true             | number    | RFQ-Kaufbetrag                                                                                                        |
| inputApyDefinition | true             | string    | Der zugrunde liegende Code ist Enum, der angibt, wie der Eingabe-APY berechnet wird: OptimusDefaultAPY, BinanceDntAPY, AaveLendingAPY |
| protectedApy       | false            | number    | Garantierte jährliche Rendite (leer für RISKY, erforderlich für protected)                                                      |
| fundingApy         | false            | number    | AAVE jährliche Rendite (leer für RISKY, erforderlich für protected)                                                            |
| takerWallet        | false            | string    | Öffentliche Adressinformationen der Wallet des Anfragenden                                                                           |

**Antwortparameter**

| **Feldname**                | **Typ**      | **Beschreibung**                                 |
| --------------------------- | ------------ | ------------------------------------------------ |
| code                        | int          | 0 zeigt an, dass das Rückgaberesultat normal ist  |
| message                     | string       | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value                       | object       | Wie unten gezeigt                                 |

Objekt

| **Feldname**                | **Typ**      | **Beschreibung**                                                                                   |
| --------------------------- | ------------ | ------------------------------------------------------------------------------------------------- |
| rfqId                       | number       | RFQ-ID                                                                                             |
| vault                       | string       | Vertragsadresse                                                                                   |
| chainId                     | int          | Chain-ID                                                                                           |
| riskType                    | string       | Risikotyp: PROTECTED, RISKY                                                                        |
| direction                   | string       | BULLISH, BEARISH                                                                                  |
| forCcy                      | string       | Basiswährung                                                                                       |
| domCcy                      | string       | Währungspaar                                                                                       |
| depositCcy                  | string       | Zeichnungswährung                                                                                 |
| lowerBarrier                | number       | Untere Preisgrenze                                                                                 |
| upperBarrier                | number       | Obere Preisgrenze                                                                                 |
| depositAmount               | number       | RFQ-Kaufbetrag                                                                                     |
| expiry                      | number       | Ablaufzeitstempel (z. B. 1672387200)                                                              |
| timestamp                   | number       | Auslösezeitpunkt der aktuellen Preisgestaltung; der nächste Beobachtungsbeginn wird basierend auf dieser Logik berechnet |
| feeRate                     | object       | Handels- und Abwicklungsgebührensatz (optional)                                                  |
| leverageInfo                | object       | Kreditinformationen (optional)                                                                     |
| relevantDollarPrices        | list[object] | Tokenpreis, der für die RCH-Preiskonversion und die Airdrop-Berechnung erforderlich ist (optional) |
| amounts                     | object       | Berechnete Beträge (optional)                                                                      |
| apyInfo                     | object       | Annualisierte Informationen, verfügbar für Nicht-Surge-Produkte (optional)                       |
| oddsInfo                    | object       | Quoteninformationen, verfügbar für Surge-Produkte (optional)                                     |
| quote                       | object       |                                                                                                   |
| \> quoteId                 | number       |                                                                                                   |
| \> anchorPrices            | list[string] | 20000000000, 30000000000                                                                          |
| \> makerCollateral          | string       | Betrag der Maker-Sicherheit                                                                         |
| \> totalCollateral          | string       | Gesamtbetrag der Sicherheit (Taker + Maker)                                                       |
| \> collateralAtRisk        | string       | Erforderlich, wenn garantiert (optional)                                                          |
| \> makerBalanceThreshold    | string       | Maker-Balance-Schwelle                                                                             |
| \> deadline                 | number       | Ablaufzeitstempel (z. B. 1672387200)                                                              |
| \> makerWallet              | string       | Wallet des Makers (optional)                                                                       |
| \> signature                | string       | Unterschrift (optional)                                                                            |

### Smart Trend Gewinnwahrscheinlichkeit

Wahrscheinlichkeit, innerhalb der Grenzen bis zum Ablauf zu bleiben

```
GET rfq/smart-trend/winning-probabilities
```

**Eingabeparameter**

| **Feldname**   | **Erforderlich** | **Typ**   | **Beschreibung**                       |
| --------------- | ---------------- | --------- | -------------------------------------- |
| forCcy         | true             | string    | Basiswährung                           |
| expiry         | true             | number    | Zeitstempel der zweiten Ebene für das Ablaufdatum, z.B. 1672387200 |
| lowerBarrier    | true             | number    | Untere Preisgrenze                     |
| upperBarrier    | true             | number    | Obere Preisgrenze                      |

**Antwortparameter**

| **Feldname**   | **Typ**         | **Beschreibung**                                |
| --------------- | --------------- | ------------------------------------------------ |
| code            | int             | 0 zeigt an, dass das Rückgabeergebnis normal ist |
| message         | string          | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value           | list[object]    | Wie unten gezeigt                             |

Objekt

| **Feldname**   | **Typ**         | **Beschreibung**                                |
| --------------- | --------------- | ------------------------------------------------ |
| spotPrice       | number          | Spotpreis                                      |
| timestamp       | number          |                                                  |
| probabilities   | object          | Gewinnwahrscheinlichkeit                        |


**Beispielanfrage**

```
GET rfq/smart-trend/winning-probabilities?forCcy=BTC&expiry=xxxx&lowerStrike=xxx&upperStrike=xxx
```

**Antwort**

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

## Generische Schnittstelle

### RFQ löschen

Löschen Sie die RFQ, die on-chain war

```
POST rfq/remove
```

**Eingabeparameter**

| **Feldname**   | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| rfqId          | ja          | nummer       | .       |

**Beispielanfrage**

```
{  
"rfqId":123456  
}
```

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 zeigt an, dass das Rückgabeergebnis normal ist   |
| message        | string       | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value          | object       | .                               |


### Transaktionsbenachrichtigung

```
POST rfq/trade
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| quotes | true | list[object] |   |
| \> rfqId | true | number |     |
| \> quoteId | true | number |     |
| \> txId | true | string | Transaktions-Hash |
| code | false | string | Einladungscode |
| walletType | false | string | Wallet-Typ, wie MetaMask, OKX Wallet, Coinbase usw. |

**Beispielanfrage**

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

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code           | int          | 0 zeigt an, dass das Rückgabeergebnis normal ist   |
| message        | string       | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value          | object       | .                               |

### Strike-Liste

```
GET rfq/strike-list
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| indexPrice | true | number |  |
| forCcy | false | string | Für das Handelspaar BTC-USDT geben Sie hier BTC ein; wenn nicht angegeben, wird die Standardkonfiguration verwendet. |
| domCcy | false | string | Für das Handelspaar BTC-USDT geben Sie hier USDT ein; wenn nicht angegeben, wird standardmäßig USDT verwendet. |


**Beispielanfrage**

```
{  
"indexPrice": 3750.8，  
"forCcy": "WBTC",  
}
```

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| strikes | liste[nummer] | Standardempfohlene Liste der Ausübungspreise |

### Ablauf-Liste

```
GET rfq/expiry-list
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| vault | true | string | Vertragsadresse |
| chainId | true | int | Chain-ID |


**Beispielanfrage**

```
{  
"vault": "XXXXXXXXXXXXXX",  
"chainId": 1,  
}
```

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| timestamp | nummer | Basisstartzeit in Sekundenstempel |
| expiries | liste[nummer] | Unterstützte Ablauf-Liste im Sekundenstempel, z.B. 1672387200 |

### Aave apy

```
GET rfq/aave-apy
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | Chain-ID |
| ccy | string | Einzahlungswährung |
| avgApy | string | Durchschnittlicher APY, der in den letzten 30 Tagen on-chain aufgezeichnet wurde |
| currentApy | string | Aktueller AAVE APY |
| apyUsed | string | Der tatsächlich in SofaServer verwendete APY zur Schätzung zukünftiger Zinseinnahmen |
| apyDefinition | string | Berechnungsdefinition, die dem APY entspricht |

**Beispielantwort**

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

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| chainId | true | number |     |
| ccy | true | string | USDT |
| apyDefinition | false | string | Berechnungsdefinition entsprechend dem APY; standardmäßig AaveLendingApy |


**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| chainId | number | Chain-ID |
| ccy | string | Einzahlungswährung |
| avgApy | string | Durchschnittlicher APY, der in den letzten 30 Tagen on-chain aufgezeichnet wurde |
| currentApy | string | Aktueller APY |
| apyUsed | string | Der tatsächlich in SofaServer verwendete APY zur Schätzung zukünftiger Zinserträge |
| apyDefinition | string | Berechnungsdefinition entsprechend dem APY |

**Beispielantwort**

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

### Wallet-Positionen abrufen

```
POST /rfq/position-list
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | Menge von Vertragsadressen; wenn nicht angegeben, werden alle Verträge abgefragt. |
| claimed | false | boolean | Ob es eingelöst wurde; wenn nicht angegeben, werden Positionen aller Status abgefragt. |
| expired | false | boolean | Ob es abgelaufen ist; wenn nicht angegeben, werden Positionen aller Status abgefragt. |
| concealed | false | boolean | Ob es verborgen ist; wenn nicht angegeben, werden Positionen aller Status abgefragt. |
| positiveReturn | false | boolean | Ob der Einlösungsbetrag größer als 0 ist; wenn nicht angegeben, werden alle Positionen abgefragt. |
| positiveProfit | false | boolean | Ob die Rendite das Kapital übersteigt; wenn nicht angegeben, werden alle Positionen abgefragt. |
| limit | false | Int | Anzahl der Abfragen; standardmäßig 100, maximal 300. |
| startDateTime | false | number | Entsprechender Zeitstempel in Sekunden (einschließlich), z.B. 1672387200. |
| endDateTime | false | number | Entsprechender Zeitstempel in Sekunden (einschließlich), z.B. 1672387200. |
| orderBy | false | string | "createdAt" oder "return", Sortiermethode: "createdAt" (Aktualisierungszeit, Standard) oder "return" (Renditen). |
| orderDirection | false | string | "desc" oder "asc", standardmäßig "desc" (absteigende Reihenfolge). |
| wallet | false | string | Wallet-Adresse (wenn leer, werden alle Wallet-Adressen abgefragt). |

Hinweis: Wenn die Anzahl der Ergebnisse 300 innerhalb des angegebenen `startDateTime` und `endDateTime` überschreitet, kann Polling verwendet werden (in diesem Fall muss `orderBy` auf `"createdAt"` gesetzt werden), um die Abfrage fortzusetzen. Der `endDateTime`-Parameter für die n-te Polling-Anfrage sollte auf den `createdAt` des letzten Datensatzes aus dem (n-1)-ten Polling-Ergebnis gesetzt werden. Nach dem Abrufen aller Daten sollten Duplikate basierend auf dem `id`-Feld entfernt werden.

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Wie unten gezeigt |

Objekt

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string | Produkt-ID, die der Position entspricht (entspricht `productId` in den Graph-Daten). Diese ID ist innerhalb der Kette + Vault-Dimension einzigartig und kann verwendet werden, um Positionssalden abzufragen. |
| positionId | string | Positions-ID, die innerhalb der Ketten-Dimension einzigartig ist. |
| product | object | Produktinformationen. |
| wallet | string | Wallet-Adresse. |
| createdAt | number | Entsprechender Zeitstempel in Sekunden, z.B. 1672387200. |
| updatedAt | number | Entsprechender Zeitstempel in Sekunden, z.B. 1672387200. |
| claimed | boolean | Ob die Position eingelöst wurde. |
| takerAllocationRate | number | Basierend auf der Berechnungszeit schätzt den Anteil, den der Eigentümer aus dem Wettpool entnehmen kann. Vor Ablauf ist dies ein geschätzter Wert. |
| triggerTime | number | Für Rangebound die erste Ausbruchzeit; für nicht-Rangebound die Abrechnungszeit in Sekunden. |
| triggerPrice | number | Für Rangebound der erste Ausbruchspreis; für nicht-Rangebound der Abrechnungspreis. |
| feeRate | object | Gebührenrate. |
| leverageInfo | object | Kreditinformationen (optional). |
| relevantDollarPrices | list[object] | Tokenpreise, die für die RCH-Preisumrechnung und die Airdrop-Berechnung erforderlich sind. |
| amounts | object | Berechnete Beträge. |
| apyInfo | object | Annualisierte Informationen, verfügbar für Nicht-Surge-Produkte. |
| oddsInfo | object | Quoteninformationen, verfügbar für Surge-Produkte. |
| claimParams | object | Informationen zu den Anspruchsparametern. |

**Antwortbeispiel**

```
{
    "value": [{
        ...
    }]
}
```

### Wallet-Transaktionen abrufen

```
POST /rfq/transaction-list
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | Menge von Vertragsadressen; wenn nicht angegeben, werden alle Verträge abgefragt. |
| limit | false | Int | Anzahl der Abfragen; Standardwert ist 100, maximal 300. |
| startDateTime | false | number | Entsprechender Zeitstempel in Sekunden (einschließlich), z.B. 1672387200. |
| endDateTime | false | number | Entsprechender Zeitstempel in Sekunden (einschließlich), z.B. 1672387200. |
| orderDirection | false | string | "desc" oder "asc", Standardwert ist "desc" (absteigende Reihenfolge). |
| taker | false | string | Taker-Wallet-Adresse |
| maker | false | string | Maker-Wallet-Adresse |
| claimParams | false | object | Die Einlösungsparameter, die dem Feld claimParams in den Positionsdaten entsprechen. |
| hash | false | string | Transaktionshash |

Hinweis: Die Parameter Taker, Maker und claimParams können für eine gemeinsame Abfrage verwendet werden, um die Handelsaufzeichnungen zu finden, die einer Position entsprechen (eine Position kann mehreren Trades entsprechen) (nicht alle Parameter sind erforderlich). 
Wenn die Anzahl der Ergebnisse 300 innerhalb des angegebenen `startDateTime` und `endDateTime` überschreitet, kann Polling verwendet werden (in diesem Fall muss `orderBy` auf `"createdAt"` gesetzt werden), um die Abfrage fortzusetzen. Der `endDateTime`-Parameter für die n-te Polling-Anfrage sollte auf das `createdAt` des letzten Datensatzes aus dem (n-1)-ten Polling-Ergebnis gesetzt werden. Nach dem Abrufen aller Daten sollten Duplikate basierend auf dem `id`-Feld entfernt werden.

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Wie unten gezeigt |

Objekt

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | Transaktionshash |
| takerWallet | string | Adresse der Taker-Wallet. |
| makerWallet | string | Adresse der Maker-Wallet. |
| product | object | Produktinformationen. |
| createdAt | number | Entsprechender Zeitstempel in Sekunden, z.B. 1672387200. |
| takerAllocationRate | number | Schätzt basierend auf der Berechnungszeit den Anteil, den der Eigentümer aus dem Wettpool entnehmen kann. Vor Ablauf ist dies ein geschätzter Wert. |
| triggerTime | number | Für Rangebound die erste Ausbruchszeit; für nicht-Rangebound die Abrechnungszeit, in Sekunden. |
| triggerPrice | number | Für Rangebound der erste Ausbruchspreis; für nicht-Rangebound der Abrechnungspreis. |
| feeRate | object | Gebührenrate. |
| leverageInfo | object | Kreditinformationen (optional). |
| relevantDollarPrices | list[object] | Tokenpreise, die für die RCH-Preisumrechnung und die Airdrop-Berechnung erforderlich sind. |
| amounts | object | Berechnete Beträge. |
| apyInfo | object | Annualisierte Informationen, verfügbar für nicht-Surge-Produkte. |
| oddsInfo | object | Quoteninformationen, verfügbar für Surge-Produkte. |

**Beispielantwort**

```
{
    "value": [{
        ...
    }]
}
```

### Verlustpositionen verbergen

```
POST rfq/position/conceal
```

**Eingabeparameter**

| **Feldname**   | **Erforderlich** | **Typ** | **Beschreibung**                |
| --------------- | ---------------- | ------- | ------------------------------- |
| chainId        | true             | int     | Chain-ID                        |
| positionIds     | true             | List[string] | Liste der positionIds, bis zu 20 |

**Beispielanfrage**

```
{
    "positionIds": [
        "aaaa","bbbb"
    ],
    "chainId": 1
}
```

**Antwortparameter**

| **Feldname**   | **Typ**        | **Beschreibung**                                |
| --------------- | -------------- | ---------------------------------------------- |
| code            | int            | 0 zeigt an, dass das Rückgabeergebnis normal ist |
| message         | string         | Fehlermeldung, die im Falle einer Ausnahme zurückgegeben wird |
| value           | object         | .                               |


### Ausstehende Transaktionen abrufen

Dies dient hauptsächlich der Behebung des Problems der Synchronisierung der zugrunde liegenden Positionsdaten. Die hier abgerufenen Daten können verschwinden (aufgrund von Blockchain-Gabelwettbewerb).

```
POST rfq/transactions/pending
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| chainId | true | Int |  |
| vaults | false | list[string] | Menge von Vertragsadressen; wenn nicht angegeben, werden alle Verträge abgefragt. |
| taker | false | string | Taker-Wallet-Adresse |
| maker | false | string | Maker-Wallet-Adresse |

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Wie unten gezeigt |

Objekt

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| id | string |  |
| hash | string | Transaktionshash |
| takerWallet | string | Taker-Wallet-Adresse. |
| makerWallet | string | Maker-Wallet-Adresse. |
| product | object | Produktinformationen. |
| createdAt | number | Entsprechender Zeitstempel in Sekunden, z.B. 1672387200. |
| takerAllocationRate | number | Basierend auf der Berechnungszeit schätzt es den Anteil, den der Eigentümer aus dem Wettpool entnehmen kann. Vor Ablauf ist dies ein geschätzter Wert. |
| triggerTime | number | Für Rangebound die erste Ausbruchzeit; für nicht-Rangebound die Abrechnungszeit in Sekunden. |
| triggerPrice | number | Für Rangebound der erste Ausbruchspreis; für nicht-Rangebound der Abrechnungspreis. |
| feeRate | object | Gebührenrate. |
| leverageInfo | object | Kreditinformationen (optional). |
| relevantDollarPrices | list[object] | Tokenpreise, die für die RCH-Preisumrechnung und die Airdrop-Berechnung erforderlich sind. |
| amounts | object | Berechnete Beträge. |
| apyInfo | object | Annualisierte Informationen, verfügbar für nicht-Surge-Produkte. |
| oddsInfo | object | Quoteninformationen, verfügbar für Surge-Produkte. |


### RCH Airdrop-Historie abrufen

```
GET rfq/airdrop/history
```

**Eingabeparameter**

| **Feldname** | **Erforderlich** | **Typ** | **Beschreibung** |
| --- | --- | --- | --- |
| wallet | true | string | Wallet-Adresse |
| startDateTime | true | number | Entsprechender Zeitstempel in Sekunden (einschließlich), z.B. 1672387200. |
| endDateTime | true | number | Entsprechender Zeitstempel in Sekunden (einschließlich), z.B. 1672387200. |
| orderBy | false | string | "dateTime" oder "rch", Sortiermethode: "dateTime" (Airdrop-Zeit, Standard) oder "rch" (Betrag). |
| orderDirection | false | string | "desc" oder "asc", standardmäßig "desc" (absteigende Reihenfolge). |

**Antwortparameter**

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| code | number |  |
| message | string |  |
| value | list[object] | Wie unten gezeigt |

Objekt

| **Feldname** | **Typ**     | **Beschreibung**                                |
| -------------- | ------------ | ---------------------------------------------- |
| dateTime | long | Der aktuelle Zeitstempel in Sekunden, zum Beispiel 1672387200. |
| wallet | string | Wallet-Adresse |
| volume | string | Handelsvolumen |
| rch | string | Airdrop-Betrag, der Rohwert, muss durch 1e18 geteilt werden |
| merkleProof | string | Merkle-Beweis  |
