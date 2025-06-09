# APIs ouvertes pour le Market Maker

## Spécification

### Préfixe

1. Pour garantir la sécurité des transactions, utilisez HTTPS pour la transmission.
2. JSON est le format d'échange de données.
3. L'encodage des caractères UTF-8 est appliqué universellement.
4. L'algorithme de signature de l'interface utilise HMAC-SHA256.
5. Un horodatage en millisecondes UNIX est utilisé, représentant le nombre de millisecondes depuis le 1er janvier 1970, 0:00:00.

### Paramètres

#### Demande

| nom          | type   | remarque                                               |
| ------------ | ------ | ------------------------------------------------------ |
| H-Request-Id | string | [En-tête]ID de demande, unique                         |
| H-Api-Key    | string | [En-tête]Clé API                                      |
| H-Timestamp  | long   | [En-tête]Horodatage valide, par exemple, 1672387200000 |
| H-Nonce      | string | [En-tête]Chaîne aléatoire                             |
| Authorization| string | [En-tête]Signature, par exemple : [mm_id]-hmac-sha256 signature |

#### Réponse

| nom     | type    | remarque             |
| ------- | ------- | --------------------- |
| code    | integer | [Corps]Code d'erreur  |
| message | string  | [Corps]Raison de l'erreur |
| value   | T       | [Corps]Résultat       |

### Génération de la signature

Notre plateforme RFQ nécessite des signatures de partenaires pour approuver les demandes, suivies d'une procédure de vérification. Un échec de vérification entraînera un rejet de la plateforme avec une réponse `401` Non autorisé.

#### Construction de la chaîne de signature :

La chaîne de signature se compose de cinq lignes, chacune représentant un paramètre. Chaque ligne se termine par un point-virgule, y compris la dernière ligne. L'horodatage valide et le nonce de la demande sont pris respectivement des paramètres `H-Timestamp` et `H-Nonce` dans l'en-tête.

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### Remplissage de la signature

Utilisez la `SecretKey` pour chiffrer `StringToSign` en utilisant `HMAC-SHA256`.

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### Définition des en-têtes HTTP

La requête transmet la signature via l'en-tête `HTTP Authorization`. L'`Authorization header` se compose de deux parties : **type d'authentification** et **informations de signature**.

```
Authorization: AuthenticationType SignatureInformation
```

1. Type d'authentification : `[mm_id]-hmac-sha256`
2. Informations de signature : `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_Remarque :_

- La méthode de requête doit être en MAJUSCULES.
- Le `RequestBody` utilisé pour la signature doit correspondre au contenu du corps de la requête.
- Pour les requêtes GET et DELETE, le `URI` doit inclure les paramètres de la requête (par exemple, /api/v1/result?orderId=123).
  - S'il n'y a pas de corps de requête (courant pour les requêtes GET), le corps de la requête doit être une chaîne vide ("").
- Le timestamp valide (H-Timestamp) est déterminé par le demandeur ; les requêtes dépassant le timestamp valide seront rejetées par le serveur RFQ.

## RFQ

### Fournir un devis RFQ DNT

```
GET rfq/dnt/quote
```

**Paramètres**

| nom                     | requis   | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | vrai     | string | Adresse du coffre                                                  |
| chainId                 | vrai     | int    | ID de la chaîne                                                   |
| expiry                  | vrai     | long   | Temps d'expiration en secondes timestamp, par exemple, 1672387200 |
| lowerBarrier            | vrai     | number | Barrière inférieure                                               |
| upperBarrier            | vrai     | number | Barrière supérieure                                               |
| depositAmount           | vrai     | number | Dépôt                                                           |
| premiumAmount           | vrai     | number | Prime                                                            |
| protectedFundingAmount  | faux     | number | Intérêt protégé (null lorsque RISQUÉ)                            |
| deadline                | vrai     | long   | Date limite de l'offre, par exemple, 1672387200                 |
| takerWallet             | faux     | string | Adresse du portefeuille du preneur                                |
| anchorPricesDecimal     | vrai     | long   |                                                                   |
| makerCollateralDecimal  | vrai     | long   |                                                                   |
| collateralAtRiskDecimal | vrai     | long   |                                                                   |
| totalCollateralDecimal  | vrai     | long   |                                                                   |
| underlyingPair          | vrai     | string | Paire sous-jacente, par exemple BTC-USDT                         |
| trackingSource          | vrai     | string | Les sources de données sont utilisées pour suivre la valeur sous-jacente, par exemple DERIBIT |
| depositCoin             | vrai     | string | Monnaie / Pièce de la prime payée pour souscrire DNT             |
| tradingFeeRate          | vrai     | number |                                                                   |
| settlementFeeRate       | vrai     | number |                                                                   |
| riskType                | vrai     | string | Type : PROTÉGÉ, RISQUÉ                                           |

**Réponse**

| nom                       | type         | description                        |
| -------------------------- | ------------ | ---------------------------------- |
| timestamp                  | long         | Horodatage de l'offre              |
| vault                      | string       |                                    |
| chainId                    | int          |                                    |
| expiry                     | long         | Horodatage d'expiration, par exemple, 1672387200 |
| anchorPrices               | list[string] | 20000000000,30000000000            |
| makerCollateral            | string       |                                    |
| totalCollateral            | string       |                                    |
| collateralAtRisk           | chaîne       | E18                                |
| makerBalanceThreshold      | chaîne       |                                    |
| deadline                   | long         | Horodatage de la date limite de citation |
| makerWallet                | chaîne       |                                    |
| signature                  | chaîne       | .                                   |

Note :

1. $$collateralAtRisk - makerCollateral ==  premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**Exemples**

URL de la demande

```
rfq/dnt/quote
```

Paramètres

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
### Fournir une citation RFQ de tendance haussière/baissière

```

GET rfq/smart-trend/quote
```

**Paramètres**

| nom                    | requis   | type   | description                                                       |
| ---------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                  | vrai     | string | Adresse du coffre                                                  |
| chainId                | vrai     | int    | ID de la chaîne                                                   |
| expiry                 | vrai     | long   | Temps d'expiration en secondes timestamp, par exemple, 1672387200 |
| direction              | vrai     | string | BAISSIER / HAUSSIER                                              |
| lowerStrike            | vrai     | number | Barrière inférieure                                              |
| upperStrike            | vrai     | number | Barrière supérieure                                              |
| depositAmount          | vrai     | number | Dépôt                                                          |
| premiumAmount          | vrai     | number | Prime                                                           |
| protectedFundingAmount | faux     | number | Intérêt protégé (null lorsque RISQUÉ)                           |
| deadline               | vrai     | long   | Date limite de l'offre, par exemple, 1672387200                |
| takerWallet            | faux     | string | Adresse du portefeuille du preneur                               |
| anchorPricesDecimal    | vrai     | long   |                                                                 |
| makerCollateralDecimal | vrai     | long   |                                                                 |
| collateralAtRiskDecimal| vrai     | long   |                                                                 |
| totalCollateralDecimal | vrai     | long   |                                                                 |
| underlyingPair         | vrai     | string | Paire sous-jacente, par exemple BTC-USDT                        |
| trackingSource         | vrai     | string | Les sources de données sont utilisées pour suivre la valeur sous-jacente, par exemple DERIBIT |
| tradingFeeRate         | vrai     | number |                                                                 |
| settlementFeeRate      | vrai     | number |                                                                 |
| depositCoin            | vrai     | string | Monnaie / Pièce de la prime payée pour souscrire à Smart Trend  |
| riskType               | vrai     | string | Type : PROTÉGÉ, RISQUÉ                                          |

**Réponse**

| nom              | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | Horodatage de l'offre              |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | Horodatage d'expiration, par exemple, 1672387200 |
| anchorPrices     | liste[string] | 20000000000,30000000000            |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| collateralAtRisk | string       | E18                                |
| deadline         | long         | Horodatage de la date limite       |
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

### Fournir un devis RFQ double

```
GET rfq/dual/quote
```

**Paramètres**

| nom                    | requis   | type   | description                                                       |
| ---------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                  | vrai     | string | Adresse du coffre                                                 |
| chainId                | vrai     | int    | ID de la chaîne                                                  |
| expiry                 | vrai     | long   | Temps d'expiration en secondes, par exemple, 1672387200         |
| strike                 | vrai     | number | Prix d'exercice                                                  |
| type                   | vrai     | string | CALL ou PUT                                                     |
| depositAmount          | vrai     | number | Dépôt                                                           |
| deadline               | vrai     | long   | Date limite du devis, par exemple, 1672387200                   |
| refDateTime            | vrai     | long   | Temps de la demande actuelle                                     |
| takerWallet            | faux     | string | Adresse du portefeuille du preneur                               |
| anchorPriceDecimal     | vrai     | long   |                                                                 |
| makerCollateralDecimal | vrai     | long   |                                                                 |
| totalCollateralDecimal | vrai     | long   |                                                                 |
| underlyingPair         | vrai     | string | Paire sous-jacente, par exemple BTC-USDT                        |
| trackingSource         | vrai     | string | Les sources de données utilisées pour suivre la valeur sous-jacente, par exemple DERIBIT |
| depositCoin             | true     | string | Devise / Monnaie du premium payé pour souscrire au Dual             |
| depositCoinTokenAddress | true     | string |                                                                   |
| depositCoinTokenDecimal | true     | long   |                                                                   |
| tradingFeeRate          | true     | number | .                                                                  |

**Response**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | Horodatage de la citation          |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | Horodatage d'expiration, par ex., 1672387200 |
| anchorPrice      | string       | 20000000000                        |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| deadline         | long         | Horodatage de la date limite de la citation |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

## Annexe

| Code | Message                                                          |
| ---- | ---------------------------------------------------------------- |
| 1000 | erreur système.                                                 |
| 2001 | erreur de signature.                                            |
| 2002 | erreur de paramètre.                                           |
| 3001 | Les informations demandées n'existent pas.                     |
| 3002 | Le montant du dépôt est en dehors de _depositRange_.          |
| 3003 | La limite maximale des souscriptions est atteinte.             |
| 3004 | La souscription a échoué en raison d'une trop grande différence dans premiumAmount. |
| 3005 | La citation a échoué.                                          |
| 3006 | Service temporairement indisponible.                           |
| 3007 | Limite de taux d'API dépassée. Essayez de ralentir.           |
| 3100 | Échec de la création de la commande.                          |

**Fonctions requises**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$
