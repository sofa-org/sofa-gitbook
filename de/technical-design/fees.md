# Protokollgebühren

Bei SOFA verpflichten wir uns, ein faires und transparentes Finanzökosystem zu schaffen, in dem Benutzer demokratisch eine breite Palette von strukturierten Produkten zu transparenten und fairen Preisen von Market Makern erwerben können. Darüber hinaus **möchten wir als vollständig dezentrales Projekt sicherstellen, dass alle Protokollgewinne hauptsächlich mit unseren zugrunde liegenden Nutzern geteilt werden**, um eine vollständige Anreizausrichtung zu gewährleisten und nicht privilegierte Auszahlungen an Interessengruppen zu ermöglichen. Daher haben wir eine Reihe von Gebührenstrukturen mit **'Fair-Launch'-Tokenomics entwickelt, um eine angemessene Wertakkumulation sicherzustellen und die langfristige Langlebigkeit der Plattform zu unterstützen**. Es wird keine VC-Joyriding oder Exit-Liquiditätsdumping innerhalb von SOFA geben.

## Protokollgebühren

> 💰 SOFA wird 15% der Optionsprämie des Nutzers als Basis-Handelsgebühr erheben. Darüber hinaus wird im Falle einer 'gewinnenden' Auszahlung für den Nutzer eine weitere 5%ige Abwicklungsgebühr auf die gesamte Brutto-Upside-Auszahlung erhoben.

## Gebührenberechnungen

Für volle Transparenz sehen Sie bitte das folgende Beispiel, wie Gebühren und Auszahlungen in unseren Protokollen berechnet werden.

**Definitionen:**

$$Premium = Notional_{Deposit} * (1 + AAVE_{1MonthAverage} - BaseYield)^{Full Days/365} - Notional_{Deposit}$$

_MM's Sicherheiten = Vault-gesperrte Sicherheiten vom Market Maker_

**Beobachtungsfenster:**

Vom _nächsten 16:00 (UTC+8) bis zum Ablaufdatum 16:00 (UTC+8)_

**Benutzer-Auszahlungen bei Ablauf:**

1. Wenn Knocked-Out

    $$Payoff_{inUSDT} = Notional_{Deposit} + AAVEInterest_{Actual} - Premium * (1 + 0.15)$$

2. Kein Knocked-Out

    $$Payoff_{inUSDT} = Notional_{Deposit} + AAVEInterest_{Actual} + Premium * (1 - 0.15 - 0.05) + MM'sCollateral * (1 - 0.05)$$

### Numerisches Beispiel (Rangebound)

![](../../static/fees_formula.png)

### Verteilungs-Wasserfall (USDT)

![](../../static/distribution_waterfall.png)

### Native Token ($RCH) Rückkauf

> **Ausrichtung des Protokollerfolgs mit der Token-Performance**

Bei SOFA glauben wir, dass der **einfachste Weg, die Anreize von Nutzern und Hodlern in Einklang zu bringen, durch Token-Rückkäufe** erfolgt, wobei $RCH unser nativer Utility-Token ist. Wir werden **alle Protokolleinnahmen ausschließlich für $RCH-Rückkäufe verwenden**, wodurch ein positiver Kreislauf von Preisgewinnen des Tokens, die durch die Nutzung des Protokolls angetrieben werden, entsteht.

Mit einem **festen deflationären Angebot, einem methodischen Veröffentlichungszeitplan und nutzungsbasierten Airdrops ist die langfristige Wertsteigerung von $RCH weitgehend eine Funktion der Protokolleinnahmen,** die selbst ein direktes Maß für den Erfolg der Adoption ist. Darüber hinaus können wir mit **Token-Airdrops, die ausschließlich auf Protokollnutzer und Unterstützer beschränkt sind, sicherstellen, dass sie am besten von SOFAs langfristigem Erfolg profitieren, und dabei den kommerziellen Idealen von DeFi treu bleiben, 'den wahren Kernnutzern und frühen Anwendern etwas zurückzugeben'.**

**Die Logik des Token-Rückkaufs ist ein integraler Bestandteil des Smart Contracts des SOFA-Protokolls**. Die Protokolladministratoren werden diesen Prozess regelmäßig auslösen, um $RCH mit den Protokolleinnahmen über unterstützte DEX-Plattformen zu kaufen und zu verbrennen. Zerstörtes $RCH wird nicht mehr in den Umlauf gelangen, und die Gesamtmenge an $RCH wird im Laufe der Zeit mit steigender Protokollnutzung allmählich abnehmen.

Weitere Details zu unserem Tokenomics-Modell werden in einem eigenen Abschnitt weiter unten behandelt.