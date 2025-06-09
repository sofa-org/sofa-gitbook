# واجهات برمجة التطبيقات المفتوحة لصانع السوق

## المواصفات

### البادئة

1. لضمان أمان المعاملات، استخدم HTTPS للإرسال.
2. JSON هو تنسيق تبادل البيانات.
3. يتم تطبيق ترميز الأحرف UTF-8 بشكل عالمي.
4. تستخدم خوارزمية توقيع الواجهة HMAC-SHA256.
5. يتم استخدام طابع زمني بالميلي ثانية من UNIX، يمثل عدد الميلي ثانية منذ 1 يناير 1970، 0:00:00.

### المعلمات

#### الطلب

| الاسم          | النوع   | الملاحظات                                               |
| -------------- | ------ | ------------------------------------------------------- |
| H-Request-Id   | سلسلة  | [رأس] معرف الطلب، فريد                                  |
| H-Api-Key      | سلسلة  | [رأس] مفتاح API                                         |
| H-Timestamp    | طويل   | [رأس] طابع زمني صالح، على سبيل المثال، 1672387200000   |
| H-Nonce        | سلسلة  | [رأس] سلسلة عشوائية                                     |
| Authorization  | سلسلة  | [رأس] التوقيع، على سبيل المثال: [mm_id]-hmac-sha256 التوقيع |

#### الاستجابة

| الاسم    | النوع    | الملاحظات            |
| -------- | -------- | --------------------- |
| code     | عدد صحيح | [الجسم] رمز الخطأ    |
| message  | سلسلة    | [الجسم] سبب الخطأ    |
| value    | T        | [الجسم] النتيجة      |

### توليد التوقيع

تتطلب منصتنا RFQ توقيعات الشركاء للموافقة على الطلبات، تليها إجراء تحقق. ستؤدي فشل التحقق إلى رفض المنصة مع استجابة `401` غير مصرح بها.

#### بناء سلسلة التوقيع:

تتكون سلسلة التوقيع من خمسة أسطر، يمثل كل سطر معلمة. ينتهي كل سطر بفاصلة منقوطة، بما في ذلك السطر الأخير. يتم أخذ الطابع الزمني الصالح وnonce الطلب من معلمات `H-Timestamp` و `H-Nonce` في الرأس، على التوالي.

```
Timestamp;NonceStr;HTTP_METHOD();URI();RequestBody;
```

#### ملء التوقيع

استخدم `SecretKey` لتشفير `StringToSign` باستخدام `HMAC-SHA256`.

```
StringToSign = Timestamp + ";" + NonceStr + ";" + UPPERCASE(HTTP_METHOD()) + ";" + URI() + ";" + RequestBody + ";";
Signature = BASE64_STRING( HMAC-SHA256( BASE64_DECODE(SecretKey), StringToSign ) );
```

#### إعداد رؤوس HTTP

ترسل الطلب التوقيع من خلال رأس `HTTP Authorization`. يتكون `Authorization header` من جزئين: **نوع المصادقة** و **معلومات التوقيع**.

```
Authorization: AuthenticationType SignatureInformation
```

1. نوع المصادقة: `[mm_id]-hmac-sha256`
2. معلومات التوقيع: `Signature`

```
Authorization: [mm_id]-hmac-sha256 Signature
```

_ملاحظة:_

- يجب أن تكون طريقة الطلب بأحرف كبيرة.
- يجب أن يتطابق `RequestBody` المستخدم للتوقيع مع محتوى جسم الطلب.
- بالنسبة لطلبات GET و DELETE، يجب أن يتضمن `URI` معلمات الطلب (على سبيل المثال، /api/v1/result?orderId=123).
  - إذا لم يكن هناك جسم طلب (شائع لطلبات GET)، يجب أن يكون جسم الطلب سلسلة فارغة ("").
- يتم تحديد الطابع الزمني الصالح (H-Timestamp) من قبل الطالب؛ الطلبات التي تتجاوز الطابع الزمني الصالح سيتم رفضها من قبل خادم RFQ.

## RFQ

### تقديم عرض RFQ DNT

```
GET rfq/dnt/quote
```

**المعلمات**

| الاسم                    | مطلوب | النوع   | الوصف                                                       |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | true     | string | عنوان الخزنة                                                     |
| chainId                 | true     | int    | معرف السلسلة                                                          |
| expiry                  | true     | long   | وقت انتهاء الصلاحية بالثواني، على سبيل المثال، 1672387200                |
| lowerBarrier            | true     | number | الحاجز السفلي                                                     |
| upperBarrier            | true     | number | الحاجز العلوي                                                     |
| depositAmount           | true     | number | الإيداع                                                           |
| premiumAmount           | true     | number | القسط                                                           |
| protectedFundingAmount  | false    | number | الفائدة المحمية (null عند كونها عالية المخاطر)                             |
| deadline                | true     | long   | موعد انتهاء الاقتباس، على سبيل المثال، 1672387200                                  |
| takerWallet             | false    | string | عنوان محفظة المشتري                                              |
| anchorPricesDecimal     | true     | long   |                                                                   |
| makerCollateralDecimal  | true     | long   |                                                                   |
| collateralAtRiskDecimal | true     | long   |                                                                   |
| totalCollateralDecimal  | true     | long   |                                                                   |
| underlyingPair          | true     | string | الزوج الأساسي، على سبيل المثال. BTC-USDT                                    |
| trackingSource          | true     | string | مصادر البيانات المستخدمة لتتبع القيمة الأساسية، على سبيل المثال. DERIBIT |
| depositCoin             | true     | string | العملة / العملة المدفوعة كقسط للاشتراك في DNT              |
| tradingFeeRate          | true     | number |                                                                   |
| settlementFeeRate       | true     | number |                                                                   |
| riskType                | true     | string | النوع: محمي، عالي المخاطر                                            |

**الاستجابة**

| الاسم                       | النوع         | الوصف                        |
| -------------------------- | ------------ | ---------------------------------- |
| timestamp                  | long         | طابع زمني للاقتباس                    |
| vault                      | string       |                                    |
| chainId                    | int          |                                    |
| expiry                     | long         | طابع زمني لانتهاء الصلاحية، على سبيل المثال، 1672387200 |
| anchorPrices               | list[string] | 20000000000,30000000000            |
| makerCollateral            | string       |                                    |
| totalCollateral            | string       |                                    |
| collateralAtRisk           | string       | E18                                |
| makerBalanceThreshold      | string       |                                    |
| deadline                   | long         | الطابع الزمني لموعد الاقتباس      |
| makerWallet                | string       |                                    |
| signature                  | string       | .                                   |

Note:

1. $$collateralAtRisk - makerCollateral ==  premiumAmount$$
2. $$totalCollateral - makerCollateral == depositAmount$$

**أمثلة**

طلب URL

```
rfq/dnt/quote
```

المعلمات

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
### تقديم اقتباس RFQ لاتجاه الثور/الدب

```

GET rfq/smart-trend/quote
```

**المعلمات**

| الاسم                   | مطلوب   | النوع   | الوصف                                                            |
| ----------------------- | -------- | ------ | ----------------------------------------------------------------- |
| vault                   | صحيح    | سلسلة  | عنوان الخزنة                                                     |
| chainId                 | صحيح    | عدد صحيح | معرف السلسلة                                                     |
| expiry                  | صحيح    | طويل   | وقت انتهاء الصلاحية في ثوانٍ، على سبيل المثال، 1672387200      |
| direction               | صحيح    | سلسلة  | صعودي / هبوطي                                                  |
| lowerStrike             | صحيح    | عدد    | الحاجز السفلي                                                   |
| upperStrike             | صحيح    | عدد    | الحاجز العلوي                                                   |
| depositAmount           | صحيح    | عدد    | الإيداع                                                         |
| premiumAmount           | صحيح    | عدد    | القسط                                                           |
| protectedFundingAmount  | خاطئ    | عدد    | الفائدة المحمية (null عند المخاطرة)                            |
| deadline                | صحيح    | طويل   | موعد انتهاء الاقتباس، على سبيل المثال، 1672387200             |
| takerWallet             | خاطئ    | سلسلة  | عنوان محفظة المشتري                                            |
| anchorPricesDecimal     | صحيح    | طويل   |                                                                 |
| makerCollateralDecimal  | صحيح    | طويل   |                                                                 |
| collateralAtRiskDecimal | صحيح    | طويل   |                                                                 |
| totalCollateralDecimal  | صحيح    | طويل   |                                                                 |
| underlyingPair          | صحيح    | سلسلة  | الزوج الأساسي، على سبيل المثال BTC-USDT                        |
| trackingSource          | صحيح    | سلسلة  | مصادر البيانات المستخدمة لتتبع القيمة الأساسية، على سبيل المثال DERIBIT |
| tradingFeeRate          | صحيح    | عدد    |                                                                 |
| settlementFeeRate       | صحيح    | عدد    |                                                                 |
| depositCoin             | صحيح    | سلسلة  | العملة / العملة للقسط المدفوع للاشتراك في Smart Trend         |
| riskType                | صحيح    | سلسلة  | النوع: محمي، مخاطرة                                            |

**الاستجابة**

| الاسم              | النوع        | الوصف                           |
| ------------------ | ------------ | ------------------------------- |
| timestamp          | طويل        | طابع الاقتباس                  |
| vault              | سلسلة       |                                 |
| chainId            | عدد صحيح    |                                 |
| expiry             | طويل        | طابع انتهاء الصلاحية، على سبيل المثال، 1672387200 |
| anchorPrices       | قائمة[سلسلة] | 20000000000,30000000000         |
| makerCollateral     | سلسلة       |                                 |
| totalCollateral     | سلسلة       |                                 |
| collateralAtRisk | string       | E18                                |
| deadline         | long         | طابع زمني لموعد الاقتباس           |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

ملاحظة:

- $$PremiumAmount = BookingQuantity * UnitQuotePrice$$
- $$MaxPayoutAmount = BookingQuantity * (upperStrike - lowerStrike)$$
- $$MinAPYAmount == projectedFundingAmount - premiumAmount$$
- $$MaxAPYAmount == MinAPYAmount + MaxPayoutAmount$$
- $$makerCollateral = MaxPayoutAmount - premiumAmount$$
- $$collateralAtRisk = MaxPayoutAmount$$
- $$totalCollateral = depositAmount + makerCollateral$$
- $$anchorPrices = [lowerStrike, upperStrike]$$

### تقديم اقتباس RFQ مزدوج

```
GET rfq/dual/quote
```

**المعلمات**

| الاسم                    | مطلوب   | النوع   | الوصف                                                           |
| ----------------------- | -------- | ------ | --------------------------------------------------------------- |
| vault                   | true     | string | عنوان الخزنة                                                   |
| chainId                 | true     | int    | معرف السلسلة                                                  |
| expiry                  | true     | long   | وقت انتهاء الصلاحية بالثواني، على سبيل المثال، 1672387200    |
| strike                  | true     | number | سعر التنفيذ                                                   |
| type                    | true     | string | CALL أو PUT                                                   |
| depositAmount           | true     | number | الإيداع                                                       |
| deadline                | true     | long   | موعد الاقتباس، على سبيل المثال، 1672387200                  |
| refDateTime             | true     | long   | وقت الطلب الحالي                                             |
| takerWallet             | false    | string | عنوان محفظة المشتري                                          |
| anchorPriceDecimal      | true     | long   |                                                               |
| makerCollateralDecimal  | true     | long   |                                                               |
| totalCollateralDecimal  | true     | long   |                                                               |
| underlyingPair          | true     | string | الزوج الأساسي، على سبيل المثال BTC-USDT                      |
| trackingSource          | true     | string | مصادر البيانات المستخدمة لتتبع القيمة الأساسية، على سبيل المثال DERIBIT |
| depositCoin             | true     | string | العملة / العملة المدفوعة كقسط للاشتراك في دوال             |
| depositCoinTokenAddress | true     | string |                                                                   |
| depositCoinTokenDecimal | true     | long   |                                                                   |
| tradingFeeRate          | true     | number | .                                                                  |

**Response**

| name             | type         | description                        |
| ---------------- | ------------ | ---------------------------------- |
| timestamp        | long         | توقيت الاقتباس                    |
| vault            | string       |                                    |
| chainId          | int          |                                    |
| expiry           | long         | توقيت انتهاء الصلاحية، على سبيل المثال، 1672387200 |
| anchorPrice      | string       | 20000000000                        |
| makerCollateral  | string       |                                    |
| totalCollateral  | string       |                                    |
| deadline         | long         | توقيت انتهاء الاقتباس            |
| makerWallet      | string       |                                    |
| signature        | string       | .                                   |

## Appendix

| Code | Message                                                          |
| ---- | ---------------------------------------------------------------- |
| 1000 | خطأ في النظام.                                                  |
| 2001 | خطأ في التوقيع.                                                |
| 2002 | خطأ في المعاملات.                                             |
| 3001 | المعلومات المطلوبة غير موجودة.                                |
| 3002 | مبلغ الإيداع خارج _نطاق الإيداع_.                             |
| 3003 | تم الوصول إلى الحد الأقصى للاشتراكات.                        |
| 3004 | فشل الاشتراك بسبب فرق كبير في premiumAmount.                 |
| 3005 | فشل الاقتباس.                                                  |
| 3006 | لا يتم تقديم الخدمة مؤقتًا.                                   |
| 3007 | تم تجاوز حد معدل API. حاول التخفيف.                          |
| 3100 | فشل إنشاء الطلب.                                             |

**Required Functions**：

- $$premiumAmount = collateralAtRisk - makerCollateral$$
- $$depositAmount = totalCollater - makerCollateral$$
- $$bookingQuantity = collateralAtRisk$$
- $$projectedFundingAmount = projectedFundingAPY * (expDateTime - refDateTime) / 365$$
- $$Tenor = (expDateTime - refDateTime) / 365$$
- $$APYInRange = (projectedFundingAmount - premiumAmount + bookingQuantity) / tenor = makerCollateral / tenor$$
- $$APY-Protected = (projectedFundingAmount - premiumAmount) / tenor$$
