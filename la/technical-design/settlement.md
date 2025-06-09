# Considerationes de Solutione On-Chain

## Logistica Solutionis

> **Difficultates in schedulando opera in mundo DeFi**

Cum trivialibus pro systematibus conventionalibus, **retrievals data temporis-schedulati sunt difficiles pro blockchain, cum contractus intelligentes non habeant 'Cron-simile' schedulatore operum native**. Quomodo possumus mechanismum automaticum designare ad curandum ut pretia schedulentur et in tempore in crypto renoventur?

Crypto 'algo-stables' sicut Basis Cash vel ESD olim per sector DeFi pervagati sunt, adhibentes accessum intervalli 'epoch' qui _manualem_ impulsionem requireret ad operationes ad finem cuiusque epochae promovere et novum pretium solutionis constituere. **Mechanismi praemiorum in codice contractus intelligentis inclusi sunt ad coactum voluntarios 'impulsionem promovere'** et protocolla in operatione leni servare. Similiter, projectum populare vocatum Keep3r, a Andre Cronje (Yearn Finance, Fantom) conditum, tabulam operum virtualem creavit ubi quis postulata promotionis et praemia operum specificata pro hoc ipso proposito publicare posset.

Utraque solutio longe a perfectione erat, cum **ultimatum in interventionem humanam** (motivationes propriae) confideret, necnon **incertitudines de tempore exsecutionis finalis actuali**, data manualibus conatibus requisitis. Hoc ad incommoda ducere potest, ubi fixatio solutionis non renovatur usque ad post expirationem, ducens ad minus satisfactoria experientia usoris.

Possumusne creare et confidere in solutionibus off-chain quae periodicum impulsionem renovatorum pretiorum postulant? Dum possibile est, **remansimus indifferentes ad 'breviaria' centralizata** ubi protocolum periculose impedi potest per pericula sicut defectio servitoris.

Ut in sectione nostra oraculi tractatum est, SOFA **servitium Automation ChainLink ut fontem nostrorum pretiorum solutionis utitur**. Eorum servitium executionem conditionalem functionum contractuum intelligentium per reliable et decentralizatum suggestum automationis cum retis operum nodorum externorum probatorum, nunc plus quam miliarda in TVL securans, permittit.

Denique, cum **pretium solutionis finale in loco sit, usores et fabricatores mercatus possunt contractum vocare ut suum Position Token quovis tempore exurant et suum debitum solvendum accipiant**, quod calculationes a contractu intelligenti sine interruptionibus per reliable data feeds ex oraculis decentralizatis SOFA tractantur.

## Calculationes Praemiorum

Contractus ad calculandum praemia productorum omnino standardizatus est, et est Vault-indifferentis et collateral agnosticus. Proinde, **contractus calculationis definiuntur per genus structurae producti subiacente** ad calculandum praemia exemplorum. Nonnulla exempla praemiorum infra enumerantur:

### _Rangebound_

- $$Payoff{maker}=\begin{cases}X, HighSettlePrice\geq HighStrikePrice\vee LowSettlePrice\leq LowStrikePrice\\  0, HighSettlePrice<HighStrikePrice\wedge LowSettlePrice>LowStrikePrice\end{cases}$$
- $$Payoff {user}=X - Payoff {maker}$$

### _Smart Trend_

#### _Bull Trend_

- $$Payoff{maker}=\begin{cases}0, SettlePrice\geq HighStrikePrice\\
X\times\frac{HighStrikePrice-SettlePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
X, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$

#### _Bear Trend_

- $$Payoff{maker}=\begin{cases}X, SettlePrice\geq HighStrikePrice\\

X\times\frac{SettlePrice-LowStrikePrice}{HighStrikePrice-LowStrikePrice},LowStrikePrice<SettlePrice<HighStrikePrice\\
0, SettlePrice\leq LowStrikePrice\end{cases}$$

- $$Payoff {user}=X - Payoff {maker}$$