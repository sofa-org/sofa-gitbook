# Oracle-Konstruktion

In der Welt der Kryptowährungen und DeFi ist es **entscheidend, off-chain Daten zeitnah und genau zu erhalten.** Da Smart Contracts nicht direkt auf externe Systeme oder Datenquellen zugreifen können, dienen Orakel dazu, diese Lücke zu schließen, indem sie externe Informationen auf die Blockchain bringen. Der vertrauenswürdige Datenservice muss bereitgestellt werden, um vollständige Abrechnungs-Transparenz und Rückverfolgbarkeit in der Abrechnung zu gewährleisten und mögliche Streitigkeiten zu minimieren.

Der **ChainLink** Orakelservice, bekannt für seinen hohen Grad an Dezentralisierung, Zuverlässigkeit und Anpassungsfähigkeit, wurde als SOFAs primäres Mittel zum Zugriff auf off-chain Daten ausgewählt. Darüber hinaus hat die Nutzung von ChainLink-Preisdaten den **zusätzlichen Vorteil, das Protokoll gegen 'Flash Loan Angriffe' zu schützen**, während die **Quelle der Datenfeeds von renommierten Handelsplattformen wie Coinbase stammen wird**, um die Integrität der Daten zu gewährleisten.

![](../../static/chainlink.png)

SOFA nutzt [APRO Oracle](https://www.apro.com), um unsere Vermögenswerte auf mehreren Chains zu schützen. Die Unterstützung und der Schutz durch **APRO Oracle** ermöglichen es uns, die Stabilität und Sicherheit unserer On-Chain-Vermögenswerte aufrechtzuerhalten. Für weitere Informationen verweisen wir auf die [APRO Oracle-Dokumentation](https://docs.apro.com/en).

![](../../static/apro.jpg)

## Spotpreis / Preis bei Fälligkeit

Der aktuelle Preis eines zugrunde liegenden Vermögenswerts on-chain über ein Orakel zu erhalten, erfolgt typischerweise auf eine von zwei Arten:

- Aufruf von Verträgen von DEXs wie Uniswap, um den Preis oder den TWAP-Preis des zugrunde liegenden Vermögenswerts zu erhalten
- Erhalt des Vermögenspreises über Datenfeeds, die von einem Orakelservice wie ChainLink bereitgestellt werden

Angesichts der Notwendigkeit, dass der Abrechnungspreis so nah wie möglich an den CeFi-Börsenpreisen liegt (um den Nutzern oder Market Makern das Hedging zu erleichtern) und aus Sicherheitsgründen (um Manipulationen und Angriffe zu vermeiden), haben wir **ChainLinks Datenfeeds als die Orakelquelle für unseren Preis bei Fälligkeit gewählt**.

Mit ChainLinks dezentralen Datenquellen, die Preisinformationen von verschiedenen Börsen aggregieren, stellen wir sicher, dass unsere Smart Contract-Ausführungen auf aktuellen, zuverlässigen und fairen Preisdaten basieren, die öffentlich verfügbar sind.

## Berechneter Preis, abgeleitet von historischen Schlusskursen

Für Produkte wie Rangebound ist es notwendig, die höchsten und niedrigsten Preise des zugrunde liegenden Tokens von der Mint-Inception bis zur Fälligkeit zu kennen, um die endgültige Auszahlung zu berechnen. Übliche On-Chain-Daten können diese Daten nicht bereitstellen, da On-Chain-Orakel keine kontinuierlichen Zeitreihendaten liefern.

Anstatt unsere eigenen Orakelverträge als (häufigen) Zentralisierungs-Kompromiss einzusetzen, haben wir eine Lösung im **ChainLinks Functions-Produkt** gefunden. Dank der neuesten Innovationen sind wir in der Lage, off-chain APIs dezentral zu nutzen und die erhaltenen Daten über dezentrale Knoten wie einen typischen Datenfeed on-chain zu veröffentlichen.

![](../../static/KxYlbnS0IoEtX6xAxV1uB0WKsLg.png)

## Automatisierter regelmäßiger Aktualisierungsdienst

SOFA nutzt ChainLink Automation, um sicherzustellen, dass Preise automatisch aktualisiert und regelmäßig auf die Blockchain übertragen werden. ChainLink Automation bietet ein dezentrales Netzwerk, in dem Smart Contracts komplexe Aufgaben planen und automatisch ausführen können, einschließlich zeitlicher Datenaktualisierungen, Auslösen von Ereignissen und sogar kritischen Vertragsanpassungen. Dies **stellt sicher, dass die On-Chain-Preise ständig aktuell bleiben und bietet Datenvertrauen für SOFAs Netzwerk verbundener dApps**.

![](../../static/FESNbrjpEobC0DxtBz5u6Og0sgf.png)

## Dezentralisierung und Rückverfolgbarkeit

Treue zu unserem Kern-DeFi-Geist wird **SOFAs Preisdatenerfassungsprozess immer vollständig dezentralisiert sein**, und bietet vollständige Prüfungs-Rückverfolgbarkeit auf Vertragsebene. Unser Eintreten für transparente Prozesse stellt sicher, dass jeder Schritt innerhalb des Systems von den Nutzern offen überprüfbar ist, wobei Preisdaten-Details wie Anbieterquelle und Aggregationslogik jederzeit vollständig einsehbar sind.

## Kontinuierliche Innovation in Oracle-Diensten

Während ChainLink weiterhin einen Goldstandard für die Zuverlässigkeit von DeFi-Daten darstellt, erkundet SOFA kontinuierlich die neuesten Innovationen im Bereich der Oracle-Dienste und bleibt informiert. Wir sind uns der Überabhängigkeit von begrenzten Anbieterquellen bewusst und werden **bestreben, unsere Protokolleingaben so weit wie technologisch möglich zu diversifizieren**.

Langfristig strebt SOFA an, den Nutzern ein robustes, transparentes, dezentrales und nachhaltiges Datenerfassungssystem anzubieten, das die Interessen der Nutzer schützt und gleichzeitig die betriebliche Langlebigkeit gewährleistet.