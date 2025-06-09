# Open APIs pro Mercator

## Specificatio

### Praefixum

1. Ut securitatem transactionis assecurent, HTTPS ad transmissionem utere.
2. JSON est forma interchangendi data.
3. Codificatio characterum UTF-8 universaliter applicatur.
4. Algorithmi signandi interface HMAC-SHA256 utitur.
5. Timestamp millisecondum UNIX adhibetur, numerum millisecondorum a die 1 Ianuarii 1970, 0:00:00 repraesentans.

### Parametri

#### Petitio

| nomen          | genus   | nota                                                  |
| ---------------| ------- | ----------------------------------------------------- |
| H-Request-Id   | string  | [Header]Request ID, unicum                           |
| H-Api-Key      | string  | [Header]API Clavis                                   |
| H-Timestamp    | long    | [Header]Validum timestamp, e.g., 1672387200000      |
| H-Nonce        | string  | [Header]String randomis                              |
| Authorizatio   | string  | [Header]Signatura, e.g.,：[mm_id]-hmac-sha256 signatura |

#### Responsio

| nomen    | genus    | nota               |
| -------- | -------- | ------------------ |
| codex    | integer  | [Body]Error codex  |
| nuntius  | string   | [Body]Error ratio  |
| valor    | T        | [Body]Result       |

### Generatio Signaturae

Nostra RFQ suggestum signaturas sociorum ad approbandum petitiones requirit, sequitur procedura verificationis.  Defectus verificationis ad reiectionem suggesti cum responsione `401` Non auctorizato ducet.

#### Constructio signaturae string:

Signatura string quinque lineas continet, quae singula parametra repraesentant.  Unaquaeque linea cum semicolon finitur, etiam ultima linea.  Validum timestamp et nonce petitionis ex parametris `H-Timestamp` et `H-Nonce` in capite, respective, sumuntur.

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### Complens signaturam

Utere `SecretKey` ad encryptandum `StringToSign` utens `HMAC-SHA256`.

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### Statum HTTP Capita

Petitionem signaturam per `HTTP Authorization` caput transmitit. `Authorization header` duabus partibus consistit: **typus authenticationis** et **notitia signaturae**.

```
Authorization: AuthenticationType SignatureInformation
```

1. Typus Authenticationis: `[mm_id]-hmac-sha256`
2. Notitia Signaturae: `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_Nota:_

- Modus petitionis in UPPERCASE esse debet.
- `RequestBody` ad signaturam usus contentum corporis petitionis convenire debet.
- Pro petitionibus GET et DELETE, `URI` parametra petitionis includere debet (exempli gratia, /api/v1/result?orderId=123).
  - Si corpus petitionis non est (communis pro petitionibus GET), corpus petitionis esse debet stringa vacua ("").
- Validum timestamp (H-Timestamp) a petente determinatur; petitiones ultra validum timestamp a servo RFQ reiciuntur.

## RFQ

### Praebe DNT RFQ Citatum

```
GET rfq/dnt/quote
```

**Parameters**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | Adressum vault                                                     |
| chainId                 | true     | int    | ID catena                                                          |
| expiry                  | true     | long   | Tempus expirationis in secundis, e.g., 1672387200                |
| lowerBarrier            | true     | number | Inferior barrier                                                   |
| upperBarrier            | true     | number | Superior barrier                                                   |
| depositAmount           | true     | number | Depositum                                                          |
| premiumAmount           | true     | number | Premium                                                           |
| protectedFundingAmount  | false    | number | Protectum usura (null cum RISKY)                                 |
| deadline                | true     | long   | Tempus limitis, e.g., 1672387200                                  |
| takerWallet             | false    | string | Adressum wallet capientis                                         |
| anchorPricesDecimal     | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| collateralAtRiskDecimal | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | Par fundamentale, e.g. BTC-USDT                                   |
| trackingSource          | true     | string | Fontes data ad valorem fundamentale sequi, e.g. DERIBIT         |
| depositCoin             | true     | string | Moneta / Nummus premium solutum ad subscribendum DNT             |
| tradingFeeRate          | true     | number |                                                                   |
| settlementFeeRate       | true     | number |                                                                   |
| riskType                | true     | string | Typus: PROTECTED, RISKY                                          |

**Response**

| name                       | type         | description                        |
| -------------------------- | ------------ | ---------------------------------- |
| timestamp                  | long         | Tempus citationis                  |
| vault                      | string       |                                    |
| chainId                    | int          |                                    |
| expiry                     | long         | Tempus expirationis, e.g., 1672387200 |
| anchorPrices               | list[string] | 20000000000,30000000000            |
| makerCollateral            | string       |                                    |
| totalCollateral            | string       |                                    |
| collateralAtRisk           | string       | E18                                |
| makerBalanceThreshold      | string       |                                    |
| deadline                   | long         | Tempus terminus citationis         |
| makerWallet                | string       |                                    |
| signature                  | string       | .                                   |

Note:

1. $$collateralAtRisk - makerCollateral ==  premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**Examples**

Request URL

```
rfq/dnt/quote
```

Parameters

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
### Praebe Bull/Bear Trend RFQ Citationem

```

GET rfq/smart-trend/quote
```

**Parametri**

| nomen                  | necessarius | genus   | descriptio                                                       |
| ---------------------- | ----------- | ------- | ----------------------------------------------------------------- |
| vault                  | verum       | string  | Ordo vault                                                       |
| chainId                | verum       | int     | Chain ID                                                          |
| expiry                 | verum       | long    | Tempus expirationis in secundis, exempli gratia, 1672387200      |
| direction              | verum       | string  | BULLISH / BEARISH                                                 |
| lowerStrike            | verum       | numerus | Inferior barrier                                                  |
| upperStrike            | verum       | numerus | Superior barrier                                                  |
| depositAmount          | verum       | numerus | Depositum                                                         |
| premiumAmount          | verum       | numerus | Premium                                                           |
| protectedFundingAmount | falsum      | numerus | Protectum usura (null cum RISKY)                                 |
| deadline               | verum       | long    | Tempus limitis, exempli gratia, 1672387200                       |
| takerWallet            | falsum      | string  | Ordo taker wallet                                                |
| anchorPricesDecimal    | verum       | long    |                                                                 |
| makerCollateralDecimal | verum       | long    |                                                                 |
| collateralAtRiskDecimal| verum       | long    |                                                                 |
| totalCollateralDecimal | verum       | long    |                                                                 |
| underlyingPair         | verum       | string  | Par underlying, exempli gratia BTC-USDT                          |
| trackingSource         | verum       | string  | Fontes data adhibentur ad vestigandum valorem underlying, exempli gratia DERIBIT |
| tradingFeeRate         | verum       | numerus |                                                                 |
| settlementFeeRate      | verum       | numerus |                                                                 |
| depositCoin            | verum       | string  | Moneta / Nummus premium solutum ad subscribendum Smart Trend     |
| riskType               | verum       | string  | Genus: PROTECTED, RISKY                                          |

**Responsio**

| nomen            | genus         | descriptio                        |
| ---------------- | ------------- | ---------------------------------- |
| timestamp        | long          | Tempus citationis                  |
| vault            | string        |                                    |
| chainId          | int           |                                    |
| expiry           | long          | Tempus expirationis, exempli gratia, 1672387200 |
| anchorPrices     | list[string]  | 20000000000,30000000000            |
| makerCollateral  | string        |                                    |
| totalCollateral  | string        |                                    |
| collateralAtRisk | string       | E18                                |
| deadline         | long         | Tempus terminus quotationis        |
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

### Provide Dual RFQ Quote

```
GET rfq/dual/quote
```

**Parameters**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | Adres vault                                                     |
| chainId                 | true     | int    | ID catena                                                        |
| expiry                  | true     | long   | Tempus expirationis in secundis, e.g., 1672387200                |
| strike                  | true     | number | Pretium percutiendi                                             |
| type                    | true     | string | CALL aut PUT                                                    |
| depositAmount           | true     | number | Deposit                                                           |
| deadline                | true     | long   | Tempus terminus quotationis, e.g., 1672387200                   |
| refDateTime             | true     | long   | tempus petitionis currentis                                      |
| takerWallet             | false    | string | Adres wallet accipientis                                        |
| anchorPriceDecimal      | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | Par fundamentale, e.g. BTC-USDT                                  |
| trackingSource          | true     | string | Data fontes adhibentur ad vestigandum valorem fundamentale, e.g. DERIBIT |
| depositCoin             | true     | string | Moneta / Nummus praemii soluti ad subscribendum Dual             |
| depositCoinTokenAddress | true     | string |                                                                   |
| depositCoinTokenDecimal | true     | long   |                                                                   |
| tradingFeeRate          | true     | number | .                                                                  |

**Responsum**

| nomen            | genus        | descriptio                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | Tempus citationis                  |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | Tempus expirationis, e.g., 1672387200 |
| anchorPrice      | string       | 20000000000                        |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| deadline         | long         | Tempus terminus citationis        |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

## Appendix

| Codex | Nuntius                                                          |
| ----- | ---------------------------------------------------------------- |
| 1000  | error systematis.                                               |
| 2001  | error signandi.                                                |
| 2002  | error parametri.                                               |
| 3001  | Informatio petita non existit.                                |
| 3002  | Quantitas depositi extra _depositRange_ est.                  |
| 3003  | Limitem subscriptionum maximum attingitur.                    |
| 3004  | Subscriptio defecit ob nimiam differentiam in premiumAmount.  |
| 3005  | Citatio defecit.                                              |
| 3006  | Temporarily non praebere servitium.                           |
| 3007  | Limitem rate API transgressum. Conare tardius.                |
| 3100  | Creatio ordinis defecit.                                      |

**Functiones Necessariae**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$
