# Tokenomics

SOFA.org hat ein sorgfältig gestaltetes Dual-Token-Modell entwickelt, um die Tokenomics des Ökosystems zu betreiben. Der native Utility-Token des Protokolls heißt $RCH, während der Governance-Token passend $SOFA genannt wird.

Lass uns das lebendige Ökosystem von SOFA.org erkunden, das von den Tokens $RCH und $SOFA angetrieben wird.

## 🤝 **Lerne $RCH kennen: Der Utility-Token**

$RCH ist der Haupttoken von SOFA, und du kannst ihn nicht einfach frühzeitig kaufen oder von den Entwicklern erhalten. Um $RCH zu verdienen, musst du die Dienste von SOFA.org nutzen oder später an Empfehlungsprogrammen teilnehmen. Dies stellt sicher, dass nur aktive Nutzer profitieren, und vermeidet schnelle, schädliche Verkäufe, die in anderen Projekten zu sehen sind. Jeden Tag erhalten **Nutzer, die auf SOFA.org Transaktionen durchführen, $RCH basierend auf ihrer Aktivität.** Nicht einmal die Projektentwickler besitzen am Tag 1 $RCH - wie fair ist DAS? Diese faire Launch-Methode gewährleistet langfristigen Erfolg und Vorteile für die echten Nutzer der Plattform.

**Gesamtangebot:** Begrenzt auf 37.000.000 $RCH.

**Vorab-geprägtes Angebot:** 67% (25 Millionen) in einem Uniswap-Liquiditätspool mit ETH gesperrt.

**Airdrop-Angebot:** 33% werden gemäß einem festgelegten Zeitplan an die Nutzer ausgegeben.

**Einnahmenverwendung:** Alle Einnahmen des SOFA-Protokolls kaufen und verbrennen $RCH von Uniswap.

**Deflationärer Mechanismus:** Erhöhte Transaktionen führen zu mehr $RCH-Verbrennungen, was das Angebot reduziert und den Wert erhöht.

**Langfristiger Wert:** Der Preis von $RCH steigt mit der Nutzung des Protokolls, was aktiven Nutzern und Inhabern zugutekommt.

Bei der Projektstart, **werden 67% von $RCH (25 Millionen Tokens) vorab geprägt und in den Uniswap V3-Pool auf Ethereum zusammen mit ~725 ETH** (~2,7 Millionen USD) als anfänglicher Liquiditätspool platziert.

Bitte beachte, **dass diese anfängliche Liquidität niemandem gehört**. Nach der Einzahlung in den LP **werden die entsprechenden Uniswap-LP-Token umgehend zerstört, um sicherzustellen, dass der anfängliche Liquiditätspool des Tokens niemals abgehoben werden kann.**

Dies stellt sicher, dass der verfügbare $RCH-Bestand letztendlich weit weniger sein wird als das, was ursprünglich in den LP gesperrt wurde, **und damit einen effektiven Boden für den Wert von $RCH zu seinem ursprünglichen Preis setzt.**

### 🪂 Hat jemand Airdrops gesagt?!?!

Darüber hinaus **werden die verbleibenden 33% (12 Millionen Tokens) von $RCH gemäß einem vorher festgelegten Veröffentlichungszeitplan an die Nutzer des Ökosystems verteilt.**

Anfänglich **werden täglich 12.500 $RCH-Tokens über die ersten 180 Tage verteilt.** Danach werden die ausgegebenen Beträge alle 180 Tage um 20% verringert, ad infinitum.

| **Tage nach dem Start** | **Täglicher Airdrop** |
| ----------------------- | --------------------- |
| 0                       | 12.500                |
| 180                     | 10.000                |
| 360                     | 8.000                 |
| 540                     | 6.400                 |

| 720                   | 5,120             |
| 900                   | 4,096             |
| 1080                  | 3,276.8           |

### 📝Andere $RCH Hinweise

**Positive Reflexivität: **Mehr Transaktionen erhöhen den $RCH-Preis, wodurch Airdrops wertvoller werden und mehr Transaktionen gefördert werden.

**Selbstkorrekt Mechanismus: **Wenn der $RCH-Preis fällt, werden mehr Token verbrannt, was den Preis stabilisiert, bis die Transaktionen wieder zunehmen.

**Keine Exit Dumps: **Der faire Start von $RCH verhindert plötzliche große Verkaufsaktionen, da niemand $RCH beim Start erhält.

**Zukünftiges Wachstum: **Neue DeFi-Projekte können dem SOFA.org-Ökosystem als Partner beitreten.

## Erwerb von $RCH-Token & Airdrop-Mathematik

**Der beste Weg, um $RCH zu erhalten, ist zu transagieren** und Transaktionen im SOFA-Ökosystem auszuführen. Eine festgelegte Menge an $RCH wird täglich an unsere Protokollbenutzer airdropped, wobei **erhaltene Belohnungen pro-rata basierend auf den Transaktionsvolumina des Benutzers am Tag aufgeteilt werden**.

> 💰 RCH = (Vom Benutzer ausgeführte Prämie (am Tag) * [Vault-Gewicht]) / Gesamtes Vault-gewichtete Prämie, die von SOFA (am Tag) behandelt wird * 95% (dApp-Zugang)

> 💰 Gesamte Vault-gewichtete Prämie = (Gesamte Verdienste-Prämie * Verdienste-Gewicht) + (Gesamte Surge-Prämie * Surge-Gewicht)

### Annäherung der Optionsprämie

_Bitte lesen Sie den [Abschnitt zu Protokollgebühren](../technical-design/fees.md) für weitere Informationen zu Prämienberechnungen._

Für **Verdienste-basierte Produkte** wird die Optionsprämie durch die Zinseinsparungen aus Einlagen bei Aave finanziert, daher wird nur ein **kleiner Teil des gesamten Einzahlungsbetrags als 'Prämie'** für die Airdrop-Berechnungen berücksichtigt. Im Gegensatz dazu wird der **gesamte Kaufbetrag von Surge-Produkten** für die Berücksichtigung in Betracht gezogen, da er vollständig in zugrunde liegende Optionsstrategien investiert wird. Eine _ungefähre_ Schätzung der Prämie ist wie folgt:

| **Protokoll**          | **Prämie (Annäherung)**                                                              | **Kommentar**                                                                                                  |
|------------------------|--------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| Verdienen              | (Aaves Sparzins – Produktbasisrendite) * Laufzeit (in Tagen) / 365 * Einzahlungswert | SOFA verwendet nur einen Teil der Zinseinnahmen von Aave als Optionsprämie, um eine minimale Basisrendite aufrechtzuerhalten |
| Surge                  | Der gesamte gekaufte Betrag                                                          | Der gesamte Kauf wird als Optionsprämie ausgegeben, um auf hohe Aufwärtsbewegungen zu spekulieren.            |
| Automator              | Min(takerCollateral, makerCollateral) von mintProducts                               | takerCollateral = totalCollateral - makerCollateral                                                            |
| Dual(CRV)              | makerCollateral                                                                      |                                                                                                                |
| Dual(RCH, USDT, crvUSD)| Erträge aus RCH-Staking, AAVE, crvSUD-Staking / tradingFeeRate                      |                                                                                                                |

Darüber hinaus haben wir als Versuch, Aktivitäten in beiden Protokollen zu fördern, ein **Vault-Gewicht** am Ende der Formel hinzugefügt:

Vor dem 2025/02/18 8:00 UTC sind sie auf **15** für Verdienen, **15** für Hebelverdienen, **1** für Surge, **2** für RCH Surge und **2** für Automator eingestellt.

2025/02/18 8:00 UTC - 2025/04/07 8:00 UTC werden sie auf **min(Tage bis zum Ablauf, 20)** für Earn, **min(Tage bis zum Ablauf, 20)** für Leverage Earn, **1** für Surge, **2** für RCH Surge und **abs(0.5 - takerCollateral / (takerCollateral + makerCollateral)) / 0.5 * 4** für Automator festgelegt.

Danach werden sie auf **min(Tage bis zum Ablauf, 20)** für Earn, **min(Tage bis zum Ablauf, 20)** für Leverage Earn, **1** für Surge, **2** für RCH Surge, **abs(0.5 - takerCollateral / (takerCollateral + makerCollateral)) / 0.5 * 2** für Automator und **2** für Dual festgelegt.

**15%** des Automator-Airdrops werden dem RCH Deployer für RCH-Brennzwecke in Rechnung gestellt. Schließlich wird ein endgültiger **95%** universeller Anpassungsfaktor angewendet, um den Airdrop-Haarschnitt zu berücksichtigen, der an den [dApp-Broker](../INTRO.md) gezahlt wird.

Mit dem Wachstum von SOFA und dem Hinzukommen zusätzlicher Protokolle können Sie erkennen, wie wichtig diese Variable zur Steigerung des TVL sein wird, und dies wird einer der entscheidenden Parameter sein, über den die Inhaber unseres $SOFA-Governance-Tokens abstimmen können.

_Hinweis: Das System erfasst tägliche Schnappschüsse. $RCH-Token sind jeden Tag um 08:00 UTC nach der Ausführung Ihres Handels zur Abholung verfügbar._

### SOFA Earn

- SOFA Earn genießt das gleiche Airdrop-Gewicht wie andere Earn-Produkte.

- Darüber hinaus erhalten alle RCH, die in SOFA Earn eingezahlt werden, eine 5% APR Einzahlungsbelohnung.

- Jede Transaktion im zugehörigen Vault (Mint/Burn/Harvest) löst die Zinsabrechnung aus, was zu kumulierten Belohnungen führt, sodass die Rendite 5% übersteigt.

- SOFA Earn-Belohnungen werden täglich durch Airdrops verteilt.

## 🤝 **Treffen Sie $SOFA: Den Governance-Token**

Der **$SOFA-Token ist der Governance-Token des SOFA.org-Ökosystems**. Als dezentrale, gemeinnützige, Open-Source-Technologieorganisation werden alle Entscheidungen innerhalb des SOFA.org-Ökosystems durch Abstimmungen der $SOFA-Token-Inhaber bestimmt. **Als reiner Governance-Token nimmt $SOFA nicht an der Gewinnverteilung innerhalb des Ökosystems teil.**

## 🎤 Die Stimme der Gemeinschaft

**Themen, die die Governance-Abstimmungen betreffen:**

1. Arten von Finanzprodukten, die an Bord genommen werden sollen
2. Bestimmung des berechtigten Korbs unterstützter Sicherheiten (USDT usw.)
3. Aufnahme neuer Partnerprotokolle in das SOFA.org-Ökosystem
4. Verteilungsmix % der täglichen $RCH-Airdrops zwischen den verschiedenen an Bord genommenen Produkten
5. Verteilungsmix % der täglichen $RCH-Airdrops unter den Ökosystemprotokollen
6. Tempo der $RCH-Airdrops
7. Und andere Entscheidungen, die im Laufe der Reifung des Ökosystems getroffen werden.