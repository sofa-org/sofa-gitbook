# Definire Tuae Rationis Praefectum

> **Quantum periculi detrimenti vis capere?**

## Protocollum Mercedis

Categoria 'Mercedis' protocollum destinatur ad depositores timidos, qui summam protectionem detrimenti capitis quaerunt.  Mercedis thesauri **stakabunt** caput depositum usorum in protocollis tutis fructiferis (ex. AAVE) ad generandum basim usurae**, et portionem illius reditus certi ad stipendium premium optionis cum mercatoribus ad potentiale augmentum.

Finis erit **maximare protectionem detrimenti**, dum adhuc aliquod augmentum potentiale servatur, si mercatus in directionem usoris favorabiliter moveatur.

Cum hoc dictum sit, quaeso nota quod **componentis designandi criticae requirunt ut fructus passivi a protocollis stakandi idoneis significanter superent pessimum casum producti, ut recte stipendium premium optionis ad generandum reditum augmenti**.  Ita, si usor ETH tenet pro USDT, necesse erit ut ETH in stETH convertat in protocollum liquidum stakandi, ut Lido, antequam stETH in thesauris protocolli clauderet ad commoditatem extra incrementi accretionis.

![](../../static/draw6.png)

![](../../static/draw7.png)

## Protocollum Surge

Pro usoribus cum maiori toleratione periculi-reward, **protocollum etiam structuras altioris reditus cum 'ante' capitis priori offert**.  Protocolla Surge solum ad usores aggressivos destinata sunt, qui reditus substantialiter altos in commutatione pro damnis capitis desiderant.

![](../../static/draw8.png)

Cum his productis basatis Surge, **thesaurus protocolli tam principium usoris quam premium mercatorum in initio negotiationis claudit, **analogum ad 'poker-ante' quoddam**. Positiones capitis claudendae _non_ ad alia protocolla restakabuntur, et fungitur ut ante obligatum contra ultimam solutionem.

Ut exemplum adhibeam productum Surge-Rangebound.  Si pretium subiecti boni stricte intra limites ad maturitatem finalem manet, usor reditum exponentially altiorum accipiet quam sub versione Mercedis.  Tamen, si contrarium eveniat, structura ante tempus terminabitur, cum principium clausum ad mercatorem transferetur ut 'victrix' huius consilii.

Iterum, **haec producta destinata sunt usoribus validam convictionem mercati habentibus, et illam fiduciam in experimentum ponere volunt in spe obtinendi valde altum rate reditus in commutatione pro damnis principii.**

## PSA: Nota Specialis de Productis Rangebound

Illustratio Pictoria Quomodo Rangebound in Pratica Operatur

Dum simplex in theoria, productum Rangebound re vera aliquas provocationes designandi habet, praesertim cum ad compatibilitatem DeFi on-chain pertinet:

- Productum _seriem_ pretiorum historicorum refert, potius quam 'punctum-in-tempore' recognitionem ad expirationem.
- Productum 'excludi' potest si limites pretiorum transgrediantur.
- Productum excludi potest _quovis momento_, sed technice impossibile est continuas referentias pretiorum on-chain per diem renovare.

  - Per extensionem, recognitiones spatii sunt "retrospicientes" ex necessitate.

The team has made the following design compromises against the preceding challenges:

1. **Daily Range Checks:**

  - In the interest of gas fees and on-chain TPS, our Rangebound product will only do a _daily_** **price check (at 4pm OTC+8) of whether the product has been knocked out over the past 24 hours.
  - Knocked-out products will terminate with no more exposure going forward.

2. **Daily Product Cycles:**

  - Consistent with the daily range checks and settlement cadence, our observation cycle will always begin from the _next _4pm (OTC+8) period
  - Nevertheless, users are free to subscribe and purchase a Rangebound product at anytime, and their Base+ Yield will start accruing immediately.

3. **Early Termination vs Final Withdrawal:**

  - Rangebound products which are 'knocked out' are effectively 'game over'; however, user deposits remain staked in Aave, and we must wait until final maturity for users to withdraw the principal in our current iteration.