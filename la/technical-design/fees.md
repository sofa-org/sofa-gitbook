# Feoda Protocolorum

Apud SOFA, nos ad creandum aequum et apertum oeconomicum systema dedicati sumus, ubi usores democratically in ampla varietate productorum structorum cum pretio aperto et aequo a fabricatoribus mercatus emere possunt. Praeterea, **ut plene decentralizatum consilium, volumus curare ut quaelibet lucrum protocolorum praecipue cum nostris usoribus subiectis communicetur**, plenum incentivum concordiam assecurans potius quam privilegium solvendum coetibus specialibus. Quare, seriem structurarum feodum excogitavimus cum **'aequae-launch' tokenomica ad curandum rectam valoris accumulationem et sustentationem diuturnitatis platformae**. Non erit gaudium VC aut exitus liquiditatis in SOFA.

## Feoda Protocolorum

> 💰 SOFA 15% premium optionis usoris ut feodum mercaturae fundamentale colliget. Praeterea, in eventu 'victricis' solutionis pro usu, ulterius 5% feodum solutionis contra totum Gross Upside Payout exigetur.

## Calculi Feodorum

Ad plenam transparentiam, quaeso vide sequentem exemplum quomodo feoda et solutiones in nostris protocolis calculentur.

**Definitiones:**

$$Premium = Notional_{Deposit} * (1 + AAVE_{1MonthAverage} - BaseYield)^{Full Days/365} - Notional_{Deposit}$$

_MM's Collateral = Collateralis Vault-Locked a Fabricatore Mercatus _

**Observatio Fenestrarum:**

Ab _proximo 16:00 (UTC+8) ad Expiry Die 16:00 (UTC+8)_

**Usoris Solutiones ad Expiry:**

1. Si Knocked-Out

    $$Payoff_{inUSDT} = Notional_{Deposit} + AAVEInterest_{Actual} - Premium * (1 + 0.15)$$

2. Non Knocked-Out

    $$Payoff_{inUSDT} = Notional_{Deposit} + AAVEInterest_{Actual} + Premium * (1 - 0.15 - 0.05) + MM'sCollateral * (1 - 0.05)$$

### Exemplum Numerale (Rangebound)

![](../../static/fees_formula.png)

### Distribuenda Cascada (USDT)

![](../../static/distribution_waterfall.png)

### Native Token ($RCH) Buyback

> **Coniungens successum protocoli cum perficiendi Token**

Apud SOFA, credimus quod **via simplicissima ad coniungendas incitamenta usorum et tenentium per token buybacks** est, cum $RCH sit noster nativus utilitatis token. Nos **omnem reditum protocoli ad $RCH buybacks excludendum compromittimus**, creando virtuosum circulum augmentorum pretii token, quae ex usu protocoli ducuntur.

Cum **fixa deflationaria copia, methodica programmatis emissionis, et usus-ducta airdrops, longum tempus valor accretionis pro $RCH plerumque est functio reditus protocoli,** qui ipse est directum mensurae successus adoptionis. Praeterea, cum **Token airdrops excludente tantum ad usores et fautores protocoli**, possumus curare ut optimam utilitatem ex longo successu SOFA capiant, manentes fideles idealibus commercialibus DeFi 'reddendi' veris usoribus et primis adoptoribus.

**Logica token buyback est pars integralis contractus intelligentis protocoli SOFA**. Administratores protocoli hanc processum regulariter incipient ad emendum et comburendum $RCH cum reditu protocoli per DEX loca supporta. $RCH destructum non amplius in circulationem intrabit, et totalis quantitas $RCH paulatim decrescet tempore cum usu protocoli crescente.

Plura de nostro Tokenomics modelo in sua propria sectione dedicata infra tractabuntur.