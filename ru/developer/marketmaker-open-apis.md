# Открытые API для маркетмейкера

## Спецификация

### Префикс

1. Для обеспечения безопасности транзакций используйте HTTPS для передачи данных.
2. JSON является форматом обмена данными.
3. Кодировка символов UTF-8 применяется повсеместно.
4. Алгоритм подписи интерфейса использует HMAC-SHA256.
5. Используется временная метка UNIX в миллисекундах, представляющая количество миллисекунд с 1 января 1970 года, 0:00:00.

### Параметры

#### Запрос

| name          | type   | remark                                                  |
| ------------- | ------ | ------------------------------------------------------- |
| H-Request-Id  | string | [Header]ID запроса, уникальный                          |
| H-Api-Key     | string | [Header]API ключ                                        |
| H-Timestamp   | long   | [Header]Действительная временная метка, например, 1672387200000 |
| H-Nonce       | string | [Header]Случайная строка                               |
| Authorization | string | [Header]Подпись, например: [mm_id]-hmac-sha256 подпись |

#### Ответ

| name    | type    | ramark             |
| ------- | ------- | ------------------ |
| code    | integer | [Body]Код ошибки   |
| message | string  | [Body]Причина ошибки |
| value   | T       | [Body]Результат     |

### Генерация подписи

Наша платформа RFQ требует подписи партнеров для одобрения запросов, за которыми следует процедура проверки. Ошибка проверки приведет к отказу платформы с ответом `401` Unauthorized.

#### Формирование строки подписи:

Строка подписи состоит из пяти строк, каждая из которых представляет параметр. Каждая строка заканчивается точкой с запятой, включая последнюю строку. Действительная временная метка и nonce запроса берутся из параметров `H-Timestamp` и `H-Nonce` в заголовке соответственно.

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### Заполнение подписи

Используйте `SecretKey` для шифрования `StringToSign` с помощью `HMAC-SHA256`.

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### Установка HTTP заголовков

Запрос передает подпись через заголовок `HTTP Authorization`. Заголовок `Authorization` состоит из двух частей: **тип аутентификации** и **информация о подписи**.

```
Authorization: AuthenticationType SignatureInformation
```

1. Тип аутентификации: `[mm_id]-hmac-sha256`
2. Информация о подписи: `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_Примечание:_

- Метод запроса должен быть написан заглавными буквами.
- `RequestBody`, используемое для подписи, должно соответствовать содержимому тела запроса.
- Для запросов GET и DELETE `URI` должен включать параметры запроса (например, /api/v1/result?orderId=123).
  - Если тело запроса отсутствует (что обычно для запросов GET), тело запроса должно быть пустой строкой ("").
- Действительная метка времени (H-Timestamp) определяется запрашивающей стороной; запросы, превышающие действительную метку времени, будут отклонены сервером RFQ.

## RFQ

### Предоставить котировку DNT RFQ

```
GET rfq/dnt/quote
```

**Параметры**

| имя                     | обязательно | тип    | описание                                                       |
| ----------------------- | ----------- | ------ | ------------------------------------------------------------- |
| vault                   | да          | строка | Адрес хранилища                                               |
| chainId                 | да          | целое  | Идентификатор цепочки                                         |
| expiry                  | да          | длинное| Время истечения в секундах, например, 1672387200            |
| lowerBarrier            | да          | число  | Нижняя граница                                               |
| upperBarrier            | да          | число  | Верхняя граница                                             |
| depositAmount           | да          | число  | Депозит                                                     |
| premiumAmount           | да          | число  | Премия                                                      |
| protectedFundingAmount  | нет         | число  | Защищенный интерес (null, если RISKY)                       |
| deadline                | да          | длинное| Срок действия котировки, например, 1672387200               |
| takerWallet             | нет         | строка | Адрес кошелька принимающей стороны                          |
| anchorPricesDecimal     | да          | длинное|                                                             |
| makerCollateralDecimal  | да          | длинное|                                                             |
| collateralAtRiskDecimal | да          | длинное|                                                             |
| totalCollateralDecimal  | да          | длинное|                                                             |
| underlyingPair          | да          | строка | Основная пара, например, BTC-USDT                            |
| trackingSource          | да          | строка | Источники данных, используемые для отслеживания основной стоимости, например, DERIBIT |
| depositCoin             | да          | строка | Валюта / монета премии, уплаченной для подписки DNT         |
| tradingFeeRate          | да          | число  |                                                             |
| settlementFeeRate       | да          | число  |                                                             |
| riskType                | да          | строка | Тип: PROTECTED, RISKY                                       |

**Ответ**

| имя                       | тип          | описание                          |
| ------------------------- | ------------ | --------------------------------- |
| timestamp                 | длинное      | Время котировки                  |
| vault                     | строка       |                                   |
| chainId                   | целое        |                                   |
| expiry                    | длинное      | Время истечения, например, 1672387200 |
| anchorPrices              | список[строка] | 20000000000,30000000000          |
| makerCollateral           | строка       |                                   |
| totalCollateral           | строка       |                                   |
| collateralAtRisk           | string       | E18                                |
| makerBalanceThreshold      | string       |                                    |
| deadline                   | long         | Временная метка срока действия заявки |
| makerWallet                | string       |                                    |
| signature                  | string       | .                                   |

Note:

1. $$collateralAtRisk - makerCollateral == premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**Примеры**

URL запроса

```
rfq/dnt/quote
```

Параметры

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
### Предоставить заявку на котировку бычьего/медвежьего тренда

```

GET rfq/smart-trend/quote
```

**Параметры**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | Адрес хранилища                                                   |
| chainId                 | true     | int    | Идентификатор цепочки                                            |
| expiry                  | true     | long   | Время истечения в секундах, например, 1672387200                |
| direction               | true     | string | БУЛЛИШ / БИРРИШ                                                 |
| lowerStrike             | true     | number | Нижняя граница                                                   |
| upperStrike             | true     | number | Верхняя граница                                                  |
| depositAmount           | true     | number | Депозит                                                         |
| premiumAmount           | true     | number | Премия                                                          |
| protectedFundingAmount  | false    | number | Защищенный интерес (null при RISKY)                             |
| deadline                | true     | long   | Срок действия котировки, например, 1672387200                   |
| takerWallet             | false    | string | Адрес кошелька принимающей стороны                               |
| anchorPricesDecimal     | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| collateralAtRiskDecimal | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | Основная пара, например, BTC-USDT                                |
| trackingSource          | true     | string | Источники данных, используемые для отслеживания основной стоимости, например, DERIBIT |
| tradingFeeRate          | true     | number |                                                                   |
| settlementFeeRate       | true     | number |                                                                   |
| depositCoin             | true     | string | Валюта / Монета премии, уплаченной за подписку на Smart Trend   |
| riskType                | true     | string | Тип: ЗАЩИЩЕННЫЙ, РИСКОВЫЙ                                       |

**Ответ**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | Время котировки                   |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | Время истечения, например, 1672387200 |
| anchorPrices     | list[string] | 20000000000,30000000000            |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| collateralAtRisk | string       | E18                                |
| deadline         | long         | Время окончания запроса              |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

Примечание:

- $$PremiumAmount = BookingQuantity * UnitQuotePrice$$
- $$MaxPayoutAmount = BookingQuantity * (upperStrike - lowerStrike)$$
- $$MinAPYAmount == projectedFundingAmount - premiumAmount$$
- $$MaxAPYAmount == MinAPYAmount + MaxPayoutAmount$$
- $$makerCollateral = MaxPayoutAmount - premiumAmount$$
- $$collateralAtRisk = MaxPayoutAmount$$
- $$totalCollateral = depositAmount + makerCollateral$$
- $$anchorPrices = [lowerStrike, upperStrike]$$

### Предоставить двойную котировку RFQ

```
GET rfq/dual/quote
```

**Параметры**

| name                    | required | type   | description                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | Адрес хранилища                                                  |
| chainId                 | true     | int    | Идентификатор цепочки                                            |
| expiry                  | true     | long   | Время истечения в секундах, например, 1672387200                |
| strike                  | true     | number | Цена исполнения                                                  |
| type                    | true     | string | CALL или PUT                                                    |
| depositAmount           | true     | number | Депозит                                                         |
| deadline                | true     | long   | Срок действия котировки, например, 1672387200                   |
| refDateTime             | true     | long   | текущее время запроса                                           |
| takerWallet             | false    | string | Адрес кошелька получателя                                       |
| anchorPriceDecimal      | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | Основная пара, например, BTC-USDT                                |
| trackingSource          | true     | string | Источники данных, используемые для отслеживания основной стоимости, например, DERIBIT |
| depositCoin             | true     | string | Валюта / Монета премии, уплаченной за подписку на Dual             |
| depositCoinTokenAddress | true     | string |                                                                   |
| depositCoinTokenDecimal | true     | long   |                                                                   |
| tradingFeeRate          | true     | number | .                                                                  |

**Response**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | Время метки котировки              |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | Время истечения, например, 1672387200 |
| anchorPrice      | string       | 20000000000                        |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| deadline         | long         | Время истечения котировки          |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

## Appendix

| Code | Message                                                          |
| ---- | ---------------------------------------------------------------- |
| 1000 | системная ошибка.                                               |
| 2001 | ошибка подписи.                                                 |
| 2002 | ошибка параметра.                                              |
| 3001 | Запрашиваемая информация не существует.                        |
| 3002 | Сумма депозита вне _depositRange_.                             |
| 3003 | Достигнут максимальный лимит подписок.                         |
| 3004 | Подписка не удалась из-за слишком большой разницы в premiumAmount. |
| 3005 | Котировка не удалась.                                         |
| 3006 | Временно не предоставляем услугу.                              |
| 3007 | Превышен лимит частоты API. Попробуйте замедлиться.           |
| 3100 | Создание заказа не удалось.                                    |

**Required Functions**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$
