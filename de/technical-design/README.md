## Produktumfang

Das SOFA-Protokoll wird **zunächst auf die Abwicklung und Tokenisierung von Krypto-Strukturprodukten fokussiert sein** als unser Proof-of-Concept. Wir gehen den „weniger beschrittenen Weg“, um zuerst die komplexesten Produkte anzugehen, wo ein erfolgreicher Einsatz eine schnelle Expansion in alle anderen Krypto-Asset-Klassen zur Folge haben wird. Das SOFA-Protokoll wird zunächst auf **Ethereum und anderen L1 EVM-Blockchains** gestartet.

## Protokoll-Workflow

![](../../static/draw4.png)

Eine hochrangige Übersicht über die typische Handelsausführung über SOFA ist wie folgt:

- Teilnehmende institutionelle Market Maker streamen kontinuierlich ausführbare Preise für strukturierte Produkte an die dApp des Protokolls
- Der Benutzer wählt ein bestimmtes strukturiertes Produkt aus und führt den Kauf basierend auf dem angezeigten Preis aus
- Die eingesetzten Vermögenswerte des Benutzers werden in den DeFi-Tresor des Produkts gesendet und gesperrt
- Die maximale Prämienexposition des Market Makers wird ebenfalls gesendet und im Tresor gesperrt
  - _Hinweis: Die Transaktion wird nicht ausgeführt, wenn eine der Seiten zu diesem Zeitpunkt ihre erforderlichen Vermögenswerte nicht bereitstellt_
- Entsprechende Positionstoken-Ansprüche (die auf Vermögensdetails verweisen) werden sowohl an den Benutzer als auch an den Market Maker geprägt, frei übertragbar wie jedes konventionelle ERC-20-Token an jedes andere Wallet-Ziel

![](../../static/TnMSbh4G7oO4fDxf7FbuTkh2sbe.png)

- **Nur für 'Earn'-Strukturen** würde das Sicherheiten im Tresor in ausgereifte und sichere Ertragsprotokolle wie Compound, AAVE usw. gestakt, um ein Basisniveau an Zinsen für die Benutzer zu verdienen
  - Auf diesen Schritt wird besonders sorgfältige Prüfung gelegt, wobei die berechtigten Ziele von den Inhabern des Governance-Tokens gewählt werden

![](../../static/Stosbf6jcoxtvyxnO3OuSb9XsPf.png)

- Schließlich wird bei Ablauf des Produkts die entsprechende Auszahlung freigegeben und kann sowohl vom Benutzer als auch vom Market Maker im Tresor beansprucht werden
  - Sollten die jeweiligen Positionstoken an eine neue Wallet übertragen werden, kann der Eigentümer der Adresse das Vermögen jederzeit nach Ablauf beanspruchen

## Token-Standards (ERC-1155)

**Das SOFA-Protokoll tokenisiert die chain-locked Positionen der Benutzer über den ERC-1155 Multi-Token-Standard und zeichnet wichtige Positionsdetails wie [Ablauf], [Ankerpreise], [Maker/Taker-Umschaltung] und andere relevante Felder für das jeweilige Instrument auf**.

![](../../static/UhIbbGdnioqb4pxRiouubc9fsOg.png)

Im Vergleich zu konventionellen Single-Asset-Token-Standards ermöglicht ERC-1155 die Erstellung und Verwaltung mehrerer Arten von Token innerhalb eines einzigen Vertrags, wobei jede Art von Token unterschiedliche Eigenschaften haben kann. Positionen mit denselben Parametern können weiterhin zusammengeführt oder aufgeteilt werden, während Übertragungen so bequem wie bei standardmäßigen ERC-20-Token ermöglicht werden. **Diese Innovation schafft ein Gleichgewicht zwischen breiter Asset-Kompatibilität, hoher Flexibilität und Gas-Effizienz**.

![](../../static/DkgrbQ5FDo5ZyxxdZvmuoixCsee.png)

Positionstoken mit dem gleichen Strike-Preis und Ablaufdatum sind fungibel wie jedes Standard-Token und können in einem einzigen Vorgang gebündelt abgerechnet werden, um erhebliche Gas-Einsparungen zu ermöglichen.