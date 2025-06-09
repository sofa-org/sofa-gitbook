# Subgraphae

Sustinemus evolutionem applicationum efficacium et responsivarum per praebitionem accessus ad data blockchain temporis realis et accurata per usum [The Graph](https://thegraph.com/)'s protocollo indexandi. Subgraphae sunt potentis instrumentum quod nobis permittit data in Ethereum recondita indexare et haec data per GraphQL interrogare.

Hic est quomodo potes subgraphas customas ad data coniuncta interroganda intra protocollo uti.

| Subgraph                | Query URL                                  |
|-------------------------|--------------------------------------------|
| SOFA Mainnet            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5Po2c7F3DiGty1pCRpsDF9yURbpapiWXmkw9ckbafLqe |
| SOFA Arbitrum           | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/HcQUG7TbdiSUpsNd4QxJ54iAHvD4TjmkUxsTfkgFdhmC |
| SOFA BSC                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/88UgiNTsJjJ15V1GXTRnUxJBza3ZsrYZyUdAiVuRwQbX |
| SOFA Polygon            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5AyRj7tY5HsXznUBCQMKuVbbdcBXQfSRQ5K77wMBwER1 |
| SOFA Sei                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/9NTKYrnPsZASfbe8gx55ZqmViWLwEZNArbkQbC6cXRVb |
| SOFA Automator Mainnet  | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/Ao7xxFupmSqH8imXCCLKK8KJnBwkMrTrkGtFfP78Mqr |
| SOFA Automator Arbitrum | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/7DKnoe1Hqek8BWrmMFKJF6RfNTH9z8th7yHqM7MCYjCt |

## Accedens Ad Subgrapham Nostram

Creavimus subgraphas specificas pro data protocollo. Hae subgraphae continent varias entitates indexatas ex blockchain, ut transactiones, rationes, et eventus specificos ad nostra DeFi producta pertinentes. Potes data in subgrapham interrogare per sequentia gradus:

### Gradus 1: Invenire Endpoint

Invenias URL endpoint GraphQL respondens subgraphae cum qua vis interagere. Hoc plerumque apparet ut nexus ad locum interretialem. Quaeso fac ut usus sis recentissimo endpoint.

### Gradus 2: Intelligere Schema

Schema describit entitates intra subgrapham et genera interrogationum quae fieri possunt. Intellectus schemae est crucialis ad scribendum interrogationes efficaciter.

### Gradus 3: Scribere Interrogationem GraphQL

Ex schema, potes incipere scribere interrogationes GraphQL ad data ex subgrapham interroganda. GraphQL permittit interrogationes valde specificas, petens tantum campos qui te interpellant. Exempli gratia, si vis interrogare informationem singularem pro unaquaque transactione, potes scribere interrogationem talem:

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

Hoc reddet transactiones et eorum relationes pro primis quinque recordibus in blockchain.

### Gradus 4: Mittere Petitionem Query

Mitte petitionem tuam query utens quolibet client HTTP qui GraphQL sustinet. Potes uti instrumento in linea, ut curl, aut bibliotheca integrata in tua applicatione, sicut apollo-client. Plerumque, petitio mittitur ut postulatio POST ad URL terminum.

Exemplum codicis utens curl est sequens:

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

Substitue URL secundum veram condicionem.

### Gradus 5: Analysare et Uti Responsione

Postquam petitio tua query missa est, servitium GraphQL responsionem in forma JSON reddet. Potes hanc data in tua applicatione processare aut directe ostendere usoribus in parte anteriori.

## Optimae Practicae

- **Utere Variabilibus Query**: Si frequenter necesse est ut queries eiusdem structurae sed cum diversis parametris perficiantur, usus variabilibus query potest facere ut facilius reutilizes tuas queries.
- **Tractare Errores**: Praeter responsiones normales, GraphQL etiam informationes de erroribus reddere potest. Fac ut codex tuus clientis has condiciones leniter tractare possit.

Sequendo superiores gradus, efficaciter uti potes The Graph ad data interroganda in protocollo SOFA ad sustinendas tuas incepta evolutionis. Quaeso refer ad reliquam documentorum nostrorum pro developer et ad fontes pertinentes Subgraph pro ulteriori informatione.
