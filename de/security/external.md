# Externe Unwägbarkeiten & Risikofaktoren

So sehr wir uns bemüht haben, die SOFA-Protokolle als 'risikofrei' zu gestalten, sind wir uns bewusst, dass unvorhergesehene Risiken vom Typ "höhere Gewalt" immer eine entfernte Möglichkeit darstellen. Darüber hinaus geht jede Designentscheidung naturgemäß mit einer Form von Kompromiss als Trade-off einher, und wir haben unser Bestes getan, um einige erwartete und 'Randfall'-Risikofaktoren hier unten aufzulisten.

1. **Ungünstige Auszahlungsergebnisse & Variabilität**
    - Bei auf Surge basierenden Produkten besteht das Risiko, das gesamte Kapital zu verlieren, wenn sich die tatsächliche Preisbewegung des Vermögenswerts ungünstig gegen die Bedingungen des Produkts bewegt (z. B. außerhalb der Barrieren).
    - Bei auf Earn basierenden Produkten hängt die genaue Zinsauszahlung der Struktur von der Preisbewegung und dem Wert des zugrunde liegenden Vermögenswerts am Fälligkeitstag ab.

2. **Überlegungen zu vorzeitigen Abhebungen**
    - Bei bestimmten Produkten mit einer Funktion zur vorzeitigen Beendigung (d. h. wenn der Preis eine der Barrieren berührt oder überschreitet, auch bekannt als Knock-out), wie beim Rangebound-Produkt, besteht die Möglichkeit, dass die Transaktion vor dem angegebenen Fälligkeitsdatum 'vorzeitig beendet' wird.
    - Sollte der Benutzer entscheiden, sein eingezahltes Kapital vorzeitig aus dem Vault abzuheben, und wir betonen - es ist eine Option, **keine** Verpflichtung - besteht die Möglichkeit, dass das zurückgegebene Kapital geringer ist als die erwartete Basisrendite und in einigen seltenen Fällen sogar unter dem ursprünglich eingezahlten Betrag liegt.
    - Die abgeleitete Auszahlung wird vollständig davon bestimmt, wann die Transaktion 'knock-out' ist im Vergleich zur Laufzeit sowie von dem aktuellen Zinssatz bei Aave für die Berechnung der Zinsen.

3. **DeFi-Erweiterungsrisiken mit externen Protokollen**
    - SOFA.org verfolgt einen sehr konservativen und maßvollen Ansatz bei der Nutzung von renommierten, sicheren und erprobten Protokollen wie Aave für unsere Staking-Aktivitäten; jedoch könnten externe Risiken und Exploits von Drittanbietern immer bei diesen etablierten Protokollen auftreten, die außerhalb unserer Kontrolle liegen.
    - Das Netzwerk der Partnerprotokolle wird voraussichtlich zusammen mit dem SOFA-Ökosystem und der DeFi-Branche im Allgemeinen wachsen.

4. **Staking-Ertragsdefizit**
    - Bei den meisten auf Earn basierenden Produkten wird die 'Basisrendite' und die eingebettete Optionsprämie direkt aus den Staking-Einnahmen von Aave finanziert. Während die SOFA-Protokolle nur einen _Teil_ der erwarteten Einnahmen zur Finanzierung der Optionsprämie als Puffer verwenden, könnte das Produkt dennoch unter einem Einkommensdefizit leiden, sollte die Aave-Rendite auf beispielsweise 0% zusammenbrechen, als unvorstellbares, 'schwarzes Schwan'-Ereignis.
    - In diesem Fall könnte die Rückzahlung an den Benutzer geringfügig geringer sein als das, was er eingezahlt hat, wobei die Differenz auf das Aave-Rendite-Defizit zurückzuführen ist.

5. **Systemische Risiken der Blockchain**
    - Das ursprüngliche SOFA-Protokoll wird zunächst auf Ethereum und EVM-kompatiblen Blockchains bereitgestellt und ist naturgemäß auf deren Proof-of-Staking-Frameworks angewiesen.
    - Ein unvorstellbarer Kompromiss in der Sicherheit der Blockchain oder andere Hacks würden die Integrität aller DeFi-Protokolle, einschließlich SOFA, gefährden.

6. **Integrität des Smart Contract Codes**
    - [SOFA.org](http://SOFA.org) hat sich mit den renommiertesten und professionellsten Sicherheitsfirmen der Branche zusammengetan und wird dies weiterhin tun, um unsere Verträge zu prüfen. Wir betrachten die Prüfung als einen fortlaufenden Prozess und werden weiterhin mehr Prüfer einladen, während sich das Protokoll weiterentwickelt.
    - Die ursprünglichen SOFA-Protokolle haben eine vollständige Kettenprüfung bestanden.
      - [Code4rena](https://code4rena.com/reports/2024-05-sofa-pro-league)
      - [Peckshield](https://github.com/peckshield/publications/blob/master/audit_reports/PeckShield-Audit-Report-Sofa-v1.0.pdf)
      - [SigmaPrime](https://github.com/sigp/public-audits/blob/master/reports/sofa/review.pdf)