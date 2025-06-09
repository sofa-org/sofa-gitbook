# Subgraphs

Wir unterstützen die Entwicklung effizienter und reaktionsschneller Anwendungen, indem wir den Zugriff auf Echtzeit- und präzise Blockchain-Daten über das Indexierungsprotokoll von [The Graph](https://thegraph.com/) bereitstellen. Subgraphs sind ein leistungsstarkes Werkzeug, das es uns ermöglicht, Daten, die auf Ethereum gespeichert sind, zu indexieren und diese Daten über GraphQL abzufragen.

Hier ist, wie Sie benutzerdefinierte Subgraphs nutzen können, um verwandte Daten innerhalb des Protokolls abzufragen.

| Subgraph                | Abfrage-URL                                  |
|-------------------------|----------------------------------------------|
| SOFA Mainnet            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5Po2c7F3DiGty1pCRpsDF9yURbpapiWXmkw9ckbafLqe |
| SOFA Arbitrum           | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/HcQUG7TbdiSUpsNd4QxJ54iAHvD4TjmkUxsTfkgFdhmC |
| SOFA BSC                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/88UgiNTsJjJ15V1GXTRnUxJBza3ZsrYZyUdAiVuRwQbX |
| SOFA Polygon            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5AyRj7tY5HsXznUBCQMKuVbbdcBXQfSRQ5K77wMBwER1 |
| SOFA Sei                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/9NTKYrnPsZASfbe8gx55ZqmViWLwEZNArbkQbC6cXRVb |
| SOFA Automator Mainnet  | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/Ao7xxFupmSqH8imXCCLKK8KJnBwkMrTrkGtFfP78Mqr |
| SOFA Automator Arbitrum | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/7DKnoe1Hqek8BWrmMFKJF6RfNTH9z8th7yHqM7MCYjCt |

## Zugriff auf Unseren Subgraph

Wir haben spezifische Subgraphs für die Daten des Protokolls erstellt. Diese Subgraphs enthalten verschiedene Entitäten, die von der Blockchain indexiert wurden, wie Transaktionen, Konten und spezifische Ereignisse, die mit unseren DeFi-Produkten verbunden sind. Sie können Daten im Subgraph über die folgenden Schritte abfragen:

### Schritt 1: Finden Sie den Endpunkt

Lokalisieren Sie die GraphQL-Endpunkt-URL, die dem Subgraph entspricht, mit dem Sie interagieren möchten. Dies erscheint typischerweise als Webadresse. Bitte stellen Sie sicher, dass Sie den aktuellsten Endpunkt verwenden.

### Schritt 2: Verstehen Sie das Schema

Das Schema beschreibt die Entitäten innerhalb des Subgraphs und die Arten von Abfragen, die durchgeführt werden können. Das Verständnis des Schemas ist entscheidend für das Schreiben effizienter Abfragen.

### Schritt 3: Schreiben Sie eine GraphQL-Abfrage

Basierend auf dem Schema können Sie beginnen, GraphQL-Abfragen zu schreiben, um Daten aus dem Subgraph abzufragen. GraphQL ermöglicht sehr spezifische Abfragen, bei denen nur die Felder angefordert werden, an denen Sie interessiert sind. Zum Beispiel, wenn Sie detaillierte Informationen zu jeder Transaktion abfragen möchten, könnten Sie eine Abfrage wie diese schreiben:

```
{
  transactions(first: 5) {
    vault
    tradingFeeRate
    totalCollateral
    timestamp
    term
    spreadAPR
    referral
    minter
    makerCollateral
    maker
    id
    hash
    expiry
    collateralAtRiskPercentage
    borrowAPR
    anchorPrices
  }
}
```

Dies wird Transaktionen und deren zugehörige Details für die ersten fünf Datensätze auf der Blockchain zurückgeben.

### Schritt 4: Senden Sie die Abfrageanfrage

Senden Sie Ihre Abfrageanfrage mit einem beliebigen HTTP-Client, der GraphQL unterstützt. Sie können ein Befehlszeilenwerkzeug wie curl oder eine in Ihre Anwendung integrierte Bibliothek wie apollo-client verwenden. Typischerweise wird die Anfrage als POST-Anfrage an die Endpunkt-URL gesendet.

Ein Beispielcode mit curl sieht wie folgt aus:

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

Ersetzen Sie die URL basierend auf der tatsächlichen Situation.

### Schritt 5: Analysieren und Verwenden Sie die Antwort

Nachdem Ihre Abfrageanfrage gesendet wurde, gibt der GraphQL-Dienst die Antwort im JSON-Format zurück. Sie können diese Daten in Ihrer Anwendung verarbeiten oder direkt auf der Benutzeroberfläche anzeigen.

## Best Practices

- **Verwenden Sie Abfragevariablen**: Wenn Sie häufig Abfragen mit derselben Struktur, aber mit unterschiedlichen Parametern durchführen müssen, kann die Verwendung von Abfragevariablen es einfacher machen, Ihre Abfragen wiederzuverwenden.
- **Fehlerbehandlung**: Neben normalen Antworten kann GraphQL auch Fehlerinformationen zurückgeben. Stellen Sie sicher, dass Ihr Client-Code diese Situationen elegant handhaben kann.

Indem Sie die obigen Schritte befolgen, können Sie The Graph effektiv nutzen, um Daten zum SOFA-Protokoll abzufragen und Ihre Entwicklungsprojekte zu unterstützen. Bitte beziehen Sie sich auf den Rest unserer Entwicklermokumentation und auf Ressourcen zu Subgraphen für weitere Informationen.
