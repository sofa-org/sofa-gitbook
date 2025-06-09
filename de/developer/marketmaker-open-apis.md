# Offene APIs für Market Maker

## Spezifikation

### Präfix

1. Um die Transaktionssicherheit zu gewährleisten, verwenden Sie HTTPS für die Übertragung.
2. JSON ist das Datenformat für den Austausch.
3. UTF-8-Zeichencodierung wird universell angewendet.
4. Der Signaturalgorithmus der Schnittstelle verwendet HMAC-SHA256.
5. Der UNIX-Millisekundenzeitstempel wird verwendet und stellt die Anzahl der Millisekunden seit dem 1. Januar 1970, 0:00:00 dar.

### Parameter

#### Anfrage

| name          | type   | remark                                                  |
| ------------- | ------ | ------------------------------------------------------- |
| H-Request-Id  | string | [Header]Anfrage-ID, eindeutig                           |
| H-Api-Key     | string | [Header]API-Schlüssel                                   |
| H-Timestamp   | long   | [Header]Gültiger Zeitstempel, z.B. 1672387200000       |
| H-Nonce       | string | [Header]Zufälliger String                               |
| Authorization | string | [Header]Signatur, z.B.: [mm_id]-hmac-sha256 Signatur  |

#### Antwort

| name    | type    | remark             |
| ------- | ------- | ------------------ |
| code    | integer | [Body]Fehlercode   |
| message | string  | [Body]Fehlergrund  |
| value   | T       | [Body]Ergebnis     |

### Signaturerstellung

Unsere RFQ-Plattform erfordert Partner-Signaturen zur Genehmigung von Anfragen, gefolgt von einem Verifizierungsverfahren. Ein Verifizierungsfehler führt zu einer Ablehnung der Plattform mit einer `401` Unauthorized-Antwort.

#### Aufbau des Signaturstrings:

Der Signaturstring besteht aus fünf Zeilen, von denen jede einen Parameter darstellt. Jede Zeile endet mit einem Semikolon, einschließlich der letzten Zeile. Der gültige Zeitstempel und der Anfrage-Nonce werden aus den Parametern `H-Timestamp` und `H-Nonce` im Header entnommen.

```
Zeitstempel;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### Ausfüllen der Signatur

Verwenden Sie den `SecretKey`, um `StringToSign` mit `HMAC-SHA256` zu verschlüsseln.

```
StringToSign = Zeitstempel + ";" + NonceStr + ";" + GROSSBUCHSTABEN(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### Einstellen der HTTP-Header

Die Anfrage überträgt die Signatur über den `HTTP Authorization`-Header. Der `Authorization-Header` besteht aus zwei Teilen: **Authentifizierungstyp** und **Signaturinformationen**.

```
Authorization: Authentifizierungstyp Signaturinformationen
```

1. Authentifizierungstyp: `[mm_id]-hmac-sha256`
2. Signaturinformationen: `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_Hinweis:_

- Die Anfragemethode sollte in GROSSBUCHSTABEN sein.
- Der `RequestBody`, der für die Signatur verwendet wird, muss mit dem Inhalt des Anfragekörpers übereinstimmen.
- Bei GET- und DELETE-Anfragen sollte die `URI` die Anfrageparameter enthalten (z. B. /api/v1/result?orderId=123).
  - Wenn es keinen Anfragekörper gibt (häufig bei GET-Anfragen), sollte der Anfragekörper ein leerer String ("") sein.
- Der gültige Zeitstempel (H-Zeitstempel) wird vom Anforderer bestimmt; Anfragen, die über den gültigen Zeitstempel hinausgehen, werden vom RFQ-Server abgelehnt.

## RFQ

### DNT RFQ-Angebot bereitstellen

```
GET rfq/dnt/quote
```

**Parameter**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | Vault-Adresse                                                     |
| chainId                 | true     | int    | Chain-ID                                                          |
| expiry                  | true     | long   | Ablaufzeit in Sekunden-Timestamp, z.B. 1672387200                |
| lowerBarrier            | true     | number | Untere Barriere                                                   |
| upperBarrier            | true     | number | Obere Barriere                                                   |
| depositAmount           | true     | number | Einzahlung                                                        |
| premiumAmount           | true     | number | Prämie                                                           |
| protectedFundingAmount  | false    | number | Geschütztes Interesse (null bei RISKY)                           |
| deadline                | true     | long   | Angebotsfrist, z.B. 1672387200                                   |
| takerWallet             | false    | string | Taker-Wallet-Adresse                                             |
| anchorPricesDecimal     | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| collateralAtRiskDecimal | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | Basis-Paar, z.B. BTC-USDT                                       |
| trackingSource          | true     | string | Datenquellen, die verwendet werden, um den zugrunde liegenden Wert zu verfolgen, z.B. DERIBIT |
| depositCoin             | true     | string | Währung / Coin der Prämie, die zur Subskription von DNT gezahlt wird |
| tradingFeeRate          | true     | number |                                                                   |
| settlementFeeRate       | true     | number |                                                                   |
| riskType                | true     | string | Typ: PROTECTED, RISKY                                            |

**Antwort**

| name                       | type         | description                        |
| -------------------------- | ------------ | ---------------------------------- |
| timestamp                  | long         | Angebotszeitstempel                |
| vault                      | string       |                                    |
| chainId                    | int          |                                    |
| expiry                     | long         | Ablaufzeitstempel, z.B. 1672387200 |
| anchorPrices               | list[string] | 20000000000,30000000000            |
| makerCollateral            | string       |                                    |
| totalCollateral            | string       |                                    |
| collateralAtRisk           | string       | E18                                |
| makerBalanceThreshold      | string       |                                    |
| deadline                   | long         | Frist für das Angebotstimestamp    |
| makerWallet                | string       |                                    |
| signature                  | string       | .                                   |

Hinweis:

1. $$collateralAtRisk - makerCollateral ==  premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**Beispiele**

Anforderungs-URL

```
rfq/dnt/quote
```

Parameter

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
### Bereitstellung des Bull/Bear Trend RFQ Angebots

```

GET rfq/smart-trend/quote
```

**Parameter**

| Name                   | erforderlich | Typ    | Beschreibung                                                       |
| ---------------------- | ----------- | ------ | ----------------------------------------------------------------- |
| vault                  | true        | string | Vault-Adresse                                                     |
| chainId                | true        | int    | Chain-ID                                                          |
| expiry                 | true        | long   | Ablaufzeit in Sekunden-Timestamp, z.B. 1672387200                |
| direction              | true        | string | BULLISH / BEARISH                                                 |
| lowerStrike            | true        | number | Untere Barriere                                                   |
| upperStrike            | true        | number | Obere Barriere                                                   |
| depositAmount          | true        | number | Einzahlung                                                        |
| premiumAmount          | true        | number | Prämie                                                           |
| protectedFundingAmount | false       | number | Geschütztes Interesse (null bei RISKY)                           |
| deadline               | true        | long   | Angebotsfrist, z.B. 1672387200                                   |
| takerWallet            | false       | string | Taker-Wallet-Adresse                                             |
| anchorPricesDecimal    | true        | long   |                                                                   |
| makerCollateralDecimal | true        | long   |                                                                   |
| collateralAtRiskDecimal| true        | long   |                                                                   |
| totalCollateralDecimal | true        | long   |                                                                   |
| underlyingPair         | true        | string | Basis-Paar, z.B. BTC-USDT                                        |
| trackingSource         | true        | string | Datenquellen, die verwendet werden, um den zugrunde liegenden Wert zu verfolgen, z.B. DERIBIT |
| tradingFeeRate         | true        | number |                                                                   |
| settlementFeeRate      | true        | number |                                                                   |
| depositCoin            | true        | string | Währung / Coin der Prämie, die zur Abonnierung von Smart Trend gezahlt wird |
| riskType               | true        | string | Typ: PROTECTED, RISKY                                            |

**Antwort**

| Name              | Typ          | Beschreibung                        |
| ----------------- | ------------ | ---------------------------------- |
| timestamp         | long         | Angebotszeitstempel                |
| vault             | string       |                                    |
| chainId           | int          |                                    |
| expiry            | long         | Ablaufzeitstempel, z.B. 1672387200 |
| anchorPrices      | list[string] | 20000000000,30000000000            |
| makerCollateral   | string       |                                    |
| totalCollateral   | string       |                                    |
| collateralAtRisk | string       | E18                                |
| deadline         | long         | Zeitstempel der Angebotsfrist      |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

Hinweis:

- $$PremiumAmount = BookingQuantity * UnitQuotePrice$$
- $$MaxPayoutAmount = BookingQuantity * (upperStrike - lowerStrike)$$
- $$MinAPYAmount == projectedFundingAmount - premiumAmount$$
- $$MaxAPYAmount == MinAPYAmount + MaxPayoutAmount$$
- $$makerCollateral = MaxPayoutAmount - premiumAmount$$
- $$collateralAtRisk = MaxPayoutAmount$$
- $$totalCollateral = depositAmount + makerCollateral$$
- $$anchorPrices = [lowerStrike, upperStrike]$$

### Bereitstellen eines Dual RFQ Angebots

```
GET rfq/dual/quote
```

**Parameter**

| name                    | required | type   | Beschreibung                                                      |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | Vault-Adresse                                                     |
| chainId                 | true     | int    | Chain-ID                                                          |
| expiry                  | true     | long   | Ablaufzeit in Sekunden, z.B. 1672387200                          |
| strike                  | true     | number | Ausübungspreis                                                   |
| type                    | true     | string | CALL oder PUT                                                    |
| depositAmount           | true     | number | Einzahlung                                                        |
| deadline                | true     | long   | Angebotsfrist, z.B. 1672387200                                   |
| refDateTime             | true     | long   | aktuelle Anforderungszeit                                        |
| takerWallet             | false    | string | Adresse der Taker-Wallet                                         |
| anchorPriceDecimal      | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | Basiswert-Paar, z.B. BTC-USDT                                    |
| trackingSource          | true     | string | Datenquellen, die verwendet werden, um den zugrunde liegenden Wert zu verfolgen, z.B. DERIBIT |
| depositCoin             | true     | string | Währung / Münze der Prämie, die für das Abonnieren von Dual bezahlt wird |
| depositCoinTokenAddress | true     | string |                                                                   |
| depositCoinTokenDecimal | true     | long   |                                                                   |
| tradingFeeRate          | true     | number | .                                                                  |

**Antwort**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | Zeitstempel des Angebots           |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | Ablaufzeitstempel, z.B. 1672387200 |
| anchorPrice      | string       | 20000000000                        |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| deadline         | long         | Zeitstempel der Angebotsfrist      |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

## Anhang

| Code | Nachricht                                                       |
| ---- | -------------------------------------------------------------- |
| 1000 | Systemfehler.                                                 |
| 2001 | Signaturfehler.                                              |
| 2002 | Parameterfehler.                                             |
| 3001 | Angeforderte Informationen existieren nicht.                 |
| 3002 | Einzahlungsbetrag liegt außerhalb des _depositRange_.         |
| 3003 | Maximales Abonnementlimit erreicht.                           |
| 3004 | Abonnement fehlgeschlagen aufgrund zu großer Differenz im premiumAmount. |
| 3005 | Angebot fehlgeschlagen.                                       |
| 3006 | Vorübergehend keinen Service anbieten.                        |
| 3007 | API-Rate-Limit überschritten. Versuchen Sie es langsamer.    |
| 3100 | Erstellung der Bestellung fehlgeschlagen.                     |

**Erforderliche Funktionen**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$
