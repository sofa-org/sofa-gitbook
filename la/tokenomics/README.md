# Tokenomics

SOFA.org diligenter consilium duorum tokenum exemplar ad operandum tokenomics ecosystematis. Nativus utilitatis token protocollo vocatur $RCH, dum token gubernationis aptissime nominatur $SOFA.

Exploramus vivum ecosystema SOFA.org, a $RCH et $SOFA tokenibus sustentatum.

## 🤝 **Invenire $RCH: Token Utilitatis**

$RCH est principale token SOFA, et non potes illud simpliciter ante emere aut a progressoribus accipere. Ut $RCH merearis, debes uti servitiis SOFA.org aut se iungere programmatibus referral posterioribus. Hoc efficit ut soli usores activos fructum capiant, vitando celeres, noxias venditiones in aliis propositis visas. Quodlibet die, **usores qui in SOFA.org negotiantur $RCH accipiunt secundum suam activitatem.** Ne progressoribus quidem projectum ullam possessionem $RCH in die 1 habent - quam aequum est ID? Haec methodus iusta launch longaevi successus et utilitates veris usoribus suggesti curat.

**Total Supply:** Limitatum ad 37,000,000 $RCH.

**Pre-Minted Supply:** 67% (25 million) in Uniswap liquidi piscina cum ETH inclusum.

**Airdrop Supply:** 33% usoribus in constituto tempore liberatum.

**Revenue Usage:** Omnis SOFA protocol revenue emit et comburit $RCH ex Uniswap.

**Deflationary Mechanism:** Auctae transactiones plures $RCH combustionem ducunt, suppletionem minuentes et valorem augentes.

**Long-Term Value:** $RCH pretium cum usu protocollo crescit, fructum ferens usoribus activis et tenentibus.

Ad launch projectum, **67% $RCH (25 million tokenum) prae-mintatum est et in Uniswap V3 piscina in Ethereum cum ~725 ETH** (~$2.7M) ut piscina initialis liquidi posita.

Nota bene, **haec initialis liquido a nemine possidetur**. Post depositum in LP, **correspondentes Uniswap LP tokena cito destruuntur, curans ut piscina initialis liquidi tokenis numquam retrahi possit.**

Hoc efficit ut $RCH fluctus available eventualiter multo minor sit quam quod initio in LP inclusum erat, **itaque efficaciter fundamentum in valore $RCH ad suum initialem pretium statuit.**

### 🪂 Dixit aliquis Airdrops?!?!

Praeterea, **reliqua 33% (12 million tokenum) $RCH airdrop ad usores ecosystematis** secundum praestitutum tempus liberandum erit.

Initio, **12,500 tokenum $RCH cotidie in primis 180 diebus airdropabuntur.** Postea, airdropata quantitas per incrementum 20% singulis 180 diebus ad infinitum decrescet.

| **Dies post Launch** | **Airdrop Cotidianus** |
| --------------------- | ----------------- |
| 0                     | 12,500            |
| 180                   | 10,000            |
| 360                   | 8,000             |
| 540                   | 6,400             |

| 720                   | 5,120             |
| 900                   | 4,096             |
| 1080                  | 3,276.8           |

### 📝Aliae Notae $RCH

**Reflexivitas Positiva: **Plures transactiones augent pretium $RCH, airdrops magis pretiosos reddentes et plures transactiones incitantes.

**Mechanismus Auto-Correctivus: **Si pretium $RCH cadit, plures tokens uruntur, pretium stabilizando usque ad transactiones iterum augendas.

**Nullae Exitus Dump: **Iusta launch $RCH impedit subitas magnas venditiones, cum nemo $RCH in launch accipit.

**Crescens Futurum: **Nova DeFi proiectorum possunt in SOFA.org ecosystema ut socii coniungi.

## Acquisitio $RCH Tokenum & Airdrop Mathematica

**Optima via ad obtinendum $RCH est transigere** et transactiones cum SOFA ecosystemate exsequi.  Statim quantitas $RCH quotidie airdropabitur ad usores nostri protocol, cum **acceptis praemiis pro-rata secundum volumina transactionum usoris in die** dividenda.

> 💰 RCH = (Premium Usoris Exsecutum (in die) * [Pondus Vault]) / Totalis Premium Pondus Vault a SOFA tractatum (in die) * 95% (dApp Access)

> 💰 Totalis Premium Pondus Vault = (Totalis Premium Meritum * Pondus Meritum) + (Totalis Premium Surge * Pondus Surge)

### Approximatio Premium Optio

_Si placet, lege sectionem [feorum protocol](../technical-design/fees.md) pro plura informatione de calculationibus premium._

Pro **productis Meritum**, cum premium optio a reditu usurae depositi ex Aave fundetur, tantum **parva pars totalis depositi usoris 'premium'** pro calculationibus airdrop considerabitur. Contra, **totum emptio quantitatis ex productis Surge** considerari poterit, cum plene ad strategias optiones subiacentes impendatur.  _Approximata_ aestimatio premium sequitur:

| **Protocol**            | **Premium (Approximatio)**                                                           | **Commentarius**                                                                                                    |
|-------------------------|---------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| Meritum                 | (Rata Salvarum Aave – Fundi Base Yield) * Maturitas (in Diebus) / 365 * Valor Depositum | SOFA tantum partem reditus usurae Aave ut premium optio ad minimum reditum conservandum utitur                  |
| Surge                   | Tota Empta Quantitas                                                                 | Tota emptio ut premium optio ad speculandum in alta ascensibus impenditur.                                      |
| Automator               | Min(takerCollateral, makerCollateral) de mintProducts                                 | takerCollateral = totalCollateral - makerCollateral                                                                |
| Dual(CRV)               | makerCollateral                                                                       |                                                                                                                    |
| Dual(RCH, USDT, crvUSD) | Reditus ex RCH Staking, AAVE, crvSUD Staking / tradingFeeRate                        |                                                                                                                    |

Praeterea, ut conemur incitare actiones per ambos protocolos, **Pondus Vault** in fine formulae inclusum est:

Ante 2025/02/18 8:00 UTC, constituta sunt **15** pro Meritum, **15** pro Leverage Meritum, **1** pro Surge, **2** pro RCH Surge et **2** pro Automator.

2025/02/18 8:00 UTC - 2025/04/07 8:00 UTC, constituti sunt ad **min(dies ad expirationem, 20)** pro Earn, **min(dies ad expirationem, 20)** pro Leverage Earn, **1** pro Surge, **2** pro RCH Surge et **abs(0.5 - takerCollateral / (takerCollateral + makerCollateral)) / 0.5 * 4** pro Automator.

Postea, constituti sunt ad **min(dies ad expirationem, 20)** pro Earn, **min(dies ad expirationem, 20)** pro Leverage Earn, **1** pro Surge, **2** pro RCH Surge, **abs(0.5 - takerCollateral / (takerCollateral + makerCollateral)) / 0.5 * 2** pro Automator et **2** pro Dual.

**15%** airdrop Automator ad RCH Deployer pro RCH comburendo impenditur. Tandem, finalis **95%** factor adjustmentis universalis applicatur ad rationem haircut airdrop ad [dApp broker](../INTRO.md).

Ut SOFA crescit et protocolla addita veniunt, potes aestimare quam momenti haec variabilis erit in impellendo TVL, et hoc erit unum ex vitalibus parametris quae a possessores $SOFA token suffragari possunt.

_Notandum: Systema quotidianos snapshots capit. $RCH tokens singulis diebus ad reclamandum praesto sunt ad UTC 08:00 post exsecutionem tuae negotiationis._

### SOFA Earn

- SOFA Earn eandem airdrop ponderationem habet ac aliae Earn producta.

- Praeterea, omnia RCH in SOFA Earn depositum recipiet 5% APR depositum praemium.

- Unaquaeque transactio in Vault associato (Mint/Burn/Harvest) interesti solutionem provocat, resultans in praemiis cumulandis, ita ut fructus 5% superet.

- SOFA Earn praemia quotidie per airdrops distribuentur.

## 🤝 **Occurrite $SOFA: Token Governance**

**$SOFA token est token governance ecosystematis SOFA.org**. Ut organizatio technologiae decentralizata, non lucrativa, et aperta, omnes decisiones intra ecosystema SOFA.org a suffragis possessores $SOFA token determinantur. **Ut purum token governance, $SOFA in ullo lucro participat intra ecosystema.**

## 🎤 Vox Communitatis

**Res quae ad Suffragia Governance pertinent:**

1. Genera productorum financialium ad onboarding
2. Determinatio coacti subsidiorum (USDT etc.) eligibilium
3. Admissionem novorum protocollorum sociorum in ecosystema SOFA.org
4. Mix distributionis % airdrops $RCH quotidianorum inter varia producta onboard
5. Mix distributionis % airdrops $RCH quotidianorum inter protocolla ecosystematis
6. Celeritas airdrops $RCH
7. Et aliae decisiones quae venturae sunt cum ecosystema maturat