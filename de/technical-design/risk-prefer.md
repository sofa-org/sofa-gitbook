# Definition Ihrer Risikopräferenz

> **Wie viel Abwärtsrisiko möchten Sie eingehen?**

## Earn-Protokoll

Die Kategorie 'Earn'-Protokoll ist für risikoscheue Einleger konzipiert, die den höchsten Abwärtsschutz ihres Kapitals suchen. Earn-Vaults werden das **eingezahlte Kapital der Nutzer in sicheren Ertragsprotokollen (z. B. AAVE) **staken**, um ein Basisniveau an Zinsen zu generieren**, und einen Teil dieses garantierten Einkommens nutzen, um die Optionsprämie mit Market Makern für Aufwärtspotenzial zu finanzieren.

Das Endergebnis wird **den Abwärtsschutz maximieren**, während dennoch ein gewisses Aufwärtspotenzial erhalten bleibt, falls sich der Markt günstig in Richtung des Nutzers bewegt.

Bitte beachten Sie, dass ein **kritischer Entwurfskomponente erfordert, dass die passiven Erträge, die von den berechtigten Staking-Protokollen angeboten werden, signifikant über der schlimmsten Auszahlung des Produkts liegen, um die Optionsprämie ordnungsgemäß zu finanzieren und die Aufwärtsrendite zu generieren**. Sollte der Nutzer ETH anstelle von USDT halten, muss er/sie die ETH in stETH in einem liquiden Staking-Protokoll wie Lido umwandeln, bevor er/sie das stETH in die Protokoll-Vaults sperrt, um von der zusätzlichen Ertragsakkumulation zu profitieren.

![](../../static/draw6.png)

![](../../static/draw7.png)

## Surge-Protokoll

Für Nutzer mit einer höheren Risiko-Rendite-Toleranz bietet das **Protokoll auch höhere Ertragsstrukturen mit einem anfänglichen Kapital 'Einsatz'**. Surge-Protokolle richten sich ausschließlich an aggressive Nutzer, die an erheblich hohen Renditen im Austausch für Kapitalverluste interessiert sind.

![](../../static/draw8.png)

Bei diesen auf Surge basierenden Produkten wird das **Protokoll-Vault sowohl das Kapital des Nutzers als auch die Prämie der Market Maker zu Beginn des Handels sperren, **analog zu einer Art 'Poker-Einsatz'**. Die gesperrten Kapitalpositionen werden _nicht_ in andere Protokolle erneut gestakt und dienen als der verpflichtete Einsatz gegen die endgültige Auszahlung.

Nehmen wir das Surge-Rangebound-Produkt als Beispiel. Sollte der Preis des zugrunde liegenden Vermögenswerts bis zur endgültigen Fälligkeit strikt innerhalb der Grenzen bleiben, erhält der Nutzer eine exponentiell höhere Rendite, als er sie unter der Earn-Version erhalten hätte. Sollte jedoch das Gegenteil eintreten, wird die Struktur vorzeitig beendet, wobei das gesperrte Kapital an den Market Maker als den 'Gewinner' dieser Strategie übertragen wird.

Noch einmal, **diese Produkte sind für Nutzer gedacht, die eine sehr starke Marktüberzeugung haben und diese Überzeugung testen möchten, in der Hoffnung, eine sehr hohe Rendite im Austausch für Kapitalverluste zu erzielen.**

## PSA: Besondere Anmerkung zu Rangebound-Produkten

Bildliche Darstellung, wie Rangebound in der Praxis funktioniert

Obwohl einfach in der Theorie, hat das Rangebound-Produkt tatsächlich einige Designherausforderungen, insbesondere in Bezug auf die On-Chain-Kompatibilität von DeFi:

- Das Produkt bezieht sich auf eine _Reihe_ historischer Preise, anstatt auf eine 'Punkt-in-der-Zeit'-Überprüfung bei Fälligkeit.
- Das Produkt kann 'ausgeschaltet' werden, wenn die Preisgrenzen überschritten werden.
- Das Produkt kann in _jeder Sekunde_ ausgeschaltet werden, aber es ist technisch unpraktisch, die On-Chain-Preisreferenzen kontinuierlich über den Tag hinweg zu aktualisieren.

  - Folglich sind Bereichsprüfungen aus Notwendigkeit "rückblickend".

Das Team hat die folgenden Designkompromisse im Hinblick auf die vorhergehenden Herausforderungen getroffen:

1. **Tägliche Bereichsprüfungen:**

  - Im Interesse der Gasgebühren und der On-Chain-TPS wird unser Rangebound-Produkt nur eine _tägliche_** **Preisprüfung (um 16 Uhr OTC+8) durchführen, um festzustellen, ob das Produkt in den letzten 24 Stunden ausgeknockt wurde.
  - Ausgeknockte Produkte werden ohne weitere Exposition in der Zukunft beendet.

2. **Tägliche Produktzyklen:**

  - In Übereinstimmung mit den täglichen Bereichsprüfungen und dem Abrechnungsrhythmus wird unser Beobachtungszyklus immer mit dem _nächsten _16 Uhr (OTC+8) Zeitraum beginnen.
  - Dennoch sind die Benutzer frei, jederzeit ein Rangebound-Produkt zu abonnieren und zu kaufen, und ihre Base+ Rendite wird sofort zu akkumulieren beginnen.

3. **Frühzeitige Beendigung vs. endgültige Abhebung:**

  - Rangebound-Produkte, die 'ausgeknockt' sind, sind effektiv 'Spiel vorbei'; jedoch bleiben die Benutzer-Einlagen in Aave gestakt, und wir müssen bis zur endgültigen Fälligkeit warten, damit die Benutzer das Kapital in unserer aktuellen Iteration abheben können.