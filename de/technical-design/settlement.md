# On-Chain-Abrechnungsüberlegungen

## Abrechnungslogistik

> **Herausforderungen bei der Jobplanung in der Welt der DeFi**

Während es für konventionelle Systeme trivial ist, **sind zeitgesteuerte Datenabrufe für Blockchain herausfordernd, da Smart Contracts keinen nativen 'Cron-ähnlichen' Job-Planer haben**. Wie können wir einen automatisierten Mechanismus entwerfen, um sicherzustellen, dass Preise rechtzeitig in der Krypto-Welt geplant und aktualisiert werden?

Krypto-'Algo-Stables' wie Basis Cash oder ESD fegten einst durch den DeFi-Sektor und setzten einen 'Epoch'-Intervallansatz ein, der einen _manuellen_ Trigger erforderte, um die Operationen am Ende jeder Epoche voranzutreiben und einen neuen Abrechnungspreis festzulegen. **Belohnungsmechanismen wurden in den Smart Contract-Code integriert, um Freiwillige zu zwingen, 'den Fortschritt auszulösen'** und die Protokolle reibungslos am Laufen zu halten. Ähnlich schuf ein beliebtes Projekt namens Keep3r, gegründet von Andre Cronje (Yearn Finance, Fantom), ein virtuelles Job-Board, auf dem man Vorausforderungsanfragen und spezifische Jobbelohnungen für diesen ganz bestimmten Zweck posten konnte.

Keine der Lösungen war perfekt, da sie **letztendlich auf menschliches Eingreifen angewiesen waren** (motivierte Eigeninteressen), ganz zu schweigen von **Unsicherheiten über die tatsächliche endgültige Ausführungszeit**, angesichts der erforderlichen manuellen Anstrengungen. Dies kann zu unangenehmen Situationen führen, in denen die Abrechnungsfixierung erst lange nach dem Ablauf aktualisiert wird, was zu einem weniger zufriedenstellenden Benutzererlebnis führt.

Könnten wir off-chain Lösungen schaffen und uns auf diese verlassen, die eine periodische Auslösung von Preisaktualisierungen erfordern? Während dies möglich ist, **bleiben wir desinteressiert an zentralen 'Abkürzungen'**, bei denen das Protokoll durch Risiken wie einen Serverausfall kritisch beeinträchtigt werden könnte.

Wie in unserem Oracle-Abschnitt behandelt, nutzt SOFA **ChainLinks Automatisierungsdienst als unsere Quelle für Abrechnungspreise**. Ihr Dienst ermöglicht die bedingte Ausführung von Smart Contract-Funktionen über eine zuverlässige und dezentrale Automatisierungsplattform mit einem bewährten Netzwerk externer Knotenoperationen, das derzeit über Milliarden in TVL sichert.

Schließlich können die **Benutzer und Market Maker, nachdem der endgültige Abrechnungspreis festgelegt ist, den Vertrag jederzeit aufrufen, um ihren Position Token zu verbrennen und ihre fällige Zahlung zu erhalten**, wobei die Berechnungen nahtlos vom Smart Contract mit zuverlässigen Datenfeeds von SOFAs dezentralen Orakeln verarbeitet werden.

## Auszahlungsberechnungen

Der Vertrag zur Berechnung der Produktauszahlungen ist vollständig standardisiert und ist Vault-unabhängig und collateral-agnostisch. Stattdessen **werden Berechnungsverträge durch den zugrunde liegenden Typ des strukturierten Produkts definiert**, um die Modellauszahlungen zu berechnen. Einige Auszahlungsbeispiele sind unten aufgeführt:

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