# الرسوم البيانية الفرعية

ندعم تطوير التطبيقات الفعالة والاستجابة من خلال توفير الوصول إلى بيانات البلوكشين الدقيقة في الوقت الحقيقي من خلال استخدام بروتوكول الفهرسة الخاص بـ [The Graph](https://thegraph.com/). الرسوم البيانية الفرعية هي أداة قوية تسمح لنا بفهرسة البيانات المخزنة على إيثيريوم واستعلام هذه البيانات عبر GraphQL.

إليك كيفية استخدام الرسوم البيانية الفرعية المخصصة لاستعلام البيانات ذات الصلة ضمن البروتوكول.

| الرسم البياني الفرعي      | رابط الاستعلام                                  |
|-------------------------|--------------------------------------------|
| الشبكة الرئيسية SOFA    | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5Po2c7F3DiGty1pCRpsDF9yURbpapiWXmkw9ckbafLqe |
| SOFA Arbitrum           | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/HcQUG7TbdiSUpsNd4QxJ54iAHvD4TjmkUxsTfkgFdhmC |
| SOFA BSC                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/88UgiNTsJjJ15V1GXTRnUxJBza3ZsrYZyUdAiVuRwQbX |
| SOFA Polygon            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5AyRj7tY5HsXznUBCQMKuVbbdcBXQfSRQ5K77wMBwER1 |
| SOFA Sei                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/9NTKYrnPsZASfbe8gx55ZqmViWLwEZNArbkQbC6cXRVb |
| الشبكة الرئيسية لمشغل SOFA  | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/Ao7xxFupmSqH8imXCCLKK8KJnBwkMrTrkGtFfP78Mqr |
| SOFA Automator Arbitrum | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/7DKnoe1Hqek8BWrmMFKJF6RfNTH9z8th7yHqM7MCYjCt |

## الوصول إلى الرسم البياني الفرعي الخاص بنا

لقد أنشأنا رسوم بيانية فرعية محددة لبيانات البروتوكول. تحتوي هذه الرسوم البيانية الفرعية على كيانات متنوعة تم فهرستها من البلوكشين، مثل المعاملات والحسابات والأحداث المحددة المتعلقة بمنتجات DeFi الخاصة بنا. يمكنك استعلام البيانات في الرسم البياني الفرعي عبر الخطوات التالية:

### الخطوة 1: العثور على نقطة النهاية

حدد رابط نقطة النهاية GraphQL المقابل للرسم البياني الفرعي الذي ترغب في التفاعل معه. يظهر هذا عادة كرابط عنوان ويب. يرجى التأكد من أنك تستخدم أحدث نقطة نهاية.

### الخطوة 2: فهم المخطط

يصف المخطط الكيانات داخل الرسم البياني الفرعي وأنواع الاستعلامات التي يمكن تنفيذها. فهم المخطط أمر حيوي لكتابة استعلامات فعالة.

### الخطوة 3: كتابة استعلام GraphQL

استنادًا إلى المخطط، يمكنك البدء في كتابة استعلامات GraphQL لاستعلام البيانات من الرسم البياني الفرعي. يسمح GraphQL باستعلامات محددة جدًا، تطلب فقط الحقول التي تهمك. على سبيل المثال، إذا كنت ترغب في استعلام معلومات مفصلة لكل معاملة، يمكنك كتابة استعلام مثل هذا:

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

سيعيد هذا المعاملات وتفاصيلها المتعلقة لأوّل خمسة سجلات على سلسلة الكتل.

### الخطوة 4: إرسال طلب الاستعلام

أرسل طلب الاستعلام الخاص بك باستخدام أي عميل HTTP يدعم GraphQL. يمكنك استخدام أداة سطر الأوامر مثل curl، أو مكتبة مدمجة في تطبيقك، مثل apollo-client. عادةً، يتم إرسال الطلب كطلب POST إلى عنوان URL النهائي.

مثال على كود باستخدام curl هو كما يلي:

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

استبدل عنوان URL بناءً على الوضع الفعلي.

### الخطوة 5: تحليل واستخدام الاستجابة

بعد إرسال طلب الاستعلام الخاص بك، ستعيد خدمة GraphQL الاستجابة بتنسيق JSON. يمكنك معالجة هذه البيانات في تطبيقك أو عرضها مباشرةً للمستخدمين على الواجهة الأمامية.

## أفضل الممارسات

- **استخدم متغيرات الاستعلام**: إذا كنت بحاجة بشكل متكرر إلى إجراء استعلامات بنفس الهيكل ولكن مع معلمات مختلفة، يمكن أن تجعل متغيرات الاستعلام من السهل إعادة استخدام استعلاماتك.
- **تعامل مع الأخطاء**: بالإضافة إلى الاستجابات العادية، يمكن أن تعيد GraphQL أيضًا معلومات الخطأ. تأكد من أن كود العميل الخاص بك يمكنه التعامل مع هذه الحالات بشكل سلس.

من خلال اتباع الخطوات المذكورة أعلاه، يمكنك استخدام The Graph بفعالية لاستعلام البيانات حول بروتوكول SOFA لدعم مشاريع تطويرك. يرجى الرجوع إلى بقية وثائق المطورين لدينا والموارد المتعلقة بـ Subgraph لمزيد من المعلومات.
