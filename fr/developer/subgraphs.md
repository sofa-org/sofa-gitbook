# Sous-graphes

Nous soutenons le développement d'applications efficaces et réactives en fournissant un accès en temps réel et précis aux données de la blockchain grâce à l'utilisation du protocole d'indexation de [The Graph](https://thegraph.com/). Les sous-graphes sont un outil puissant qui nous permet d'indexer les données stockées sur Ethereum et de les interroger via GraphQL.

Voici comment vous pouvez utiliser des sous-graphes personnalisés pour interroger des données connexes au sein du protocole.

| Sous-graphe              | URL de requête                                 |
|-------------------------|------------------------------------------------|
| SOFA Mainnet            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5Po2c7F3DiGty1pCRpsDF9yURbpapiWXmkw9ckbafLqe |
| SOFA Arbitrum           | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/HcQUG7TbdiSUpsNd4QxJ54iAHvD4TjmkUxsTfkgFdhmC |
| SOFA BSC                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/88UgiNTsJjJ15V1GXTRnUxJBza3ZsrYZyUdAiVuRwQbX |
| SOFA Polygon            | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/5AyRj7tY5HsXznUBCQMKuVbbdcBXQfSRQ5K77wMBwER1 |
| SOFA Sei                | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/9NTKYrnPsZASfbe8gx55ZqmViWLwEZNArbkQbC6cXRVb |
| SOFA Automator Mainnet  | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/Ao7xxFupmSqH8imXCCLKK8KJnBwkMrTrkGtFfP78Mqr |
| SOFA Automator Arbitrum | https://gateway.thegraph.com/api/your-api-key/subgraphs/id/7DKnoe1Hqek8BWrmMFKJF6RfNTH9z8th7yHqM7MCYjCt |

## Accéder à notre sous-graphe

Nous avons créé des sous-graphes spécifiques pour les données du protocole. Ces sous-graphes contiennent diverses entités indexées à partir de la blockchain, telles que des transactions, des comptes et des événements spécifiques liés à nos produits DeFi. Vous pouvez interroger des données dans le sous-graphe via les étapes suivantes :

### Étape 1 : Trouver l'Endpoint

Localisez l'URL de l'endpoint GraphQL correspondant au sous-graphe avec lequel vous souhaitez interagir. Cela apparaît généralement sous la forme d'un lien d'adresse web. Veuillez vous assurer que vous utilisez l'endpoint le plus à jour.

### Étape 2 : Comprendre le Schéma

Le schéma décrit les entités au sein du sous-graphe et les types de requêtes qui peuvent être effectuées. Comprendre le schéma est crucial pour écrire des requêtes efficaces.

### Étape 3 : Écrire une Requête GraphQL

En fonction du schéma, vous pouvez commencer à écrire des requêtes GraphQL pour interroger des données à partir du sous-graphe. GraphQL permet des requêtes très spécifiques, demandant uniquement les champs qui vous intéressent. Par exemple, si vous souhaitez interroger des informations détaillées pour chaque transaction, vous pourriez écrire une requête comme celle-ci :

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

Cela renverra des transactions et leurs détails associés pour les cinq premiers enregistrements sur la blockchain.

### Étape 4 : Envoyer la Demande de Requête

Envoyez votre demande de requête en utilisant n'importe quel client HTTP qui prend en charge GraphQL. Vous pouvez utiliser un outil en ligne de commande tel que curl, ou une bibliothèque intégrée dans votre application, comme apollo-client. En général, la demande est envoyée sous forme de requête POST à l'URL de point de terminaison.

Un exemple de code utilisant curl est le suivant :

```
curl -X POST -H "Content-Type: application/json" --data '{"query": "{ transactions(first: 5) { id vault totalCollateral minter timestamp } }"}'  https://api.studio.thegraph.com/query/77961/sofa-mainnet/version/latest
```

Remplacez l'URL en fonction de la situation réelle.

### Étape 5 : Analyser et Utiliser la Réponse

Après l'envoi de votre demande de requête, le service GraphQL renverra la réponse au format JSON. Vous pouvez traiter ces données dans votre application ou les afficher directement aux utilisateurs sur le front end.

## Meilleures Pratiques

- **Utilisez des Variables de Requête** : Si vous devez fréquemment effectuer des requêtes de la même structure mais avec des paramètres différents, l'utilisation de variables de requête peut faciliter la réutilisation de vos requêtes.
- **Gérez les Erreurs** : En plus des réponses normales, GraphQL peut également renvoyer des informations d'erreur. Assurez-vous que votre code client peut gérer ces situations de manière élégante.

En suivant les étapes ci-dessus, vous pouvez utiliser efficacement The Graph pour interroger des données sur le protocole SOFA afin de soutenir vos projets de développement. Veuillez consulter le reste de notre documentation pour les développeurs et les ressources liées aux Subgraphs pour plus d'informations.
