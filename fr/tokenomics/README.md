# Tokenomics

SOFA.org a méticuleusement conçu un modèle à double jeton pour faire fonctionner l'économie des tokens de l'écosystème. Le jeton utilitaire natif du protocole s'appelle $RCH, tandis que le jeton de gouvernance est judicieusement nommé $SOFA.

Explorons l'écosystème dynamique de SOFA.org, alimenté par les tokens $RCH et $SOFA.

## 🤝 **Rencontrez $RCH : Le Jeton Utilitaire**

$RCH est le jeton principal de SOFA, et vous ne pouvez pas simplement l'acheter tôt ou l'obtenir des développeurs. Pour gagner des $RCH, vous devez utiliser les services de SOFA.org ou rejoindre des programmes de parrainage ultérieurs. Cela garantit que seuls les utilisateurs actifs en bénéficient, évitant les ventes rapides et nuisibles observées dans d'autres projets. Chaque jour, **les utilisateurs qui effectuent des transactions sur SOFA.org reçoivent des $RCH en fonction de leur activité.** Même les développeurs du projet n'ont aucune propriété de $RCH le jour 1 - à quel point est-ce JUSTE ? Cette méthode de lancement équitable garantit un succès à long terme et des avantages pour les véritables utilisateurs de la plateforme.

**Offre Totale :** Limitée à 37,000,000 $RCH.

**Offre Pré-Mintée :** 67 % (25 millions) verrouillés dans un pool de liquidité Uniswap avec ETH.

**Offre Airdrop :** 33 % libérés aux utilisateurs selon un calendrier fixe.

**Utilisation des Revenus :** Tous les revenus du protocole SOFA achètent et brûlent des $RCH de Uniswap.

**Mécanisme Déflationniste :** L'augmentation des transactions entraîne plus de brûlures de $RCH, réduisant l'offre et augmentant la valeur.

**Valeur à Long Terme :** Le prix de $RCH augmente avec l'utilisation du protocole, bénéficiant aux utilisateurs actifs et aux détenteurs.

Au lancement du projet, **67 % de $RCH (25 millions de tokens) sont pré-mintés et placés dans le pool V3 de Uniswap sur Ethereum avec ~725 ETH** (~2,7 millions de dollars) comme pool de liquidité initial.

Veuillez noter, **cette liquidité initiale n'est possédée par personne**. Après le dépôt dans le LP, **les tokens LP Uniswap correspondants sont rapidement détruits, garantissant que le pool de liquidité initial du token ne peut jamais être retiré.**

Cela garantit que la flottabilité de $RCH disponible sera finalement bien inférieure à ce qui était initialement verrouillé dans le LP, **établissant ainsi un plancher effectif sur la valeur de $RCH à son prix initial.**

### 🪂 Quelqu'un a-t-il dit Airdrops?!?!

De plus, **les 33 % restants (12 millions de tokens) de $RCH seront airdropés aux utilisateurs de l'écosystème** selon un calendrier de libération prédéterminé.

Initialement, **12,500 tokens de $RCH seront airdropés quotidiennement pendant les 180 premiers jours.** Après cela, les montants airdropés diminueront de 20 % tous les 180 jours ad infinitum.

| **Jours après le Lancement** | **Airdrop Quotidien** |
| ----------------------------- | --------------------- |
| 0                             | 12,500                |
| 180                           | 10,000                |
| 360                           | 8,000                 |
| 540                           | 6,400                 |

| 720                   | 5,120             |
| 900                   | 4,096             |
| 1080                  | 3,276.8           |

### 📝Autres notes sur $RCH

**Réflexivité positive :** Plus de transactions augmentent le prix de $RCH, rendant les airdrops plus précieux et encourageant davantage de transactions.

**Mécanisme auto-correcteur :** Si le prix de $RCH baisse, plus de jetons sont brûlés, stabilisant le prix jusqu'à ce que les transactions reprennent.

**Pas de dumps à la sortie :** Le lancement équitable de $RCH empêche les ventes massives soudaines puisque personne n'obtient $RCH au lancement.

**Croissance future :** De nouveaux projets DeFi peuvent rejoindre l'écosystème SOFA.org en tant que partenaires.

## Acquisition de jetons $RCH et mathématiques des airdrops

**Le meilleur moyen d'obtenir $RCH est de transiger** et d'exécuter des transactions avec l'écosystème SOFA. Un montant fixe de $RCH sera airdropé quotidiennement à nos utilisateurs de protocole, avec **les récompenses reçues réparties au prorata en fonction des volumes de transactions de l'utilisateur au cours de la journée**.

> 💰 RCH = (Prime exécutée par l'utilisateur (du jour) * [Poids du Vault]) / Prime totale pondérée du Vault gérée par SOFA (du jour) * 95% (Accès dApp)

> 💰 Prime totale pondérée du Vault = (Prime totale gagnée * Poids de gain) + (Prime totale de Surge * Poids de Surge)

### Approximation de la prime d'option

_Veuillez vous assurer de lire la [section des frais de protocole](../technical-design/fees.md) pour plus d'informations sur les calculs de prime._

Pour les **produits basés sur Earn**, étant donné que la prime d'option sera financée par les économies d'intérêts de dépôt d'Aave, seule une **petite partie du total de dépôt de l'utilisateur sera considérée comme 'prime'** pour les calculs d'airdrop. En revanche, **le montant total d'achat des produits Surge** sera éligible à la considération, car il sera entièrement déployé dans des stratégies d'options sous-jacentes. Une estimation de prime _approximative_ est la suivante :

| **Protocole**          | **Prime (Approximation)**                                                           | **Commentaire**                                                                                                    |
|------------------------|---------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------|
| Earn                   | (Taux d'épargne d'Aave – Rendement de base du produit) * Maturité (en jours) / 365 * Valeur de dépôt | SOFA utilise seulement une partie des revenus d'intérêts d'Aave pour risquer en tant que prime d'option afin de maintenir un rendement de base minimum |
| Surge                  | Le montant total acheté                                                               | L'ensemble de l'achat est dépensé comme une prime d'option pour spéculer sur de fortes hausses.                                |
| Automator              | Min(takerCollateral, makerCollateral) de mintProducts                                 | takerCollateral = totalCollateral - makerCollateral                                                            |
| Dual(CRV)              | makerCollateral                                                                       |                                                                                                                |
| Dual(RCH, USDT, crvUSD)| Rendements du staking RCH, AAVE, staking crvSUD / tradingFeeRate                        |                                                                                                                |

De plus, dans une tentative d'inciter les activités à travers les deux protocoles, nous avons inclus un **Poids du Vault** à la fin de la formule :

Avant le 18/02/2025 à 8:00 UTC, ils sont fixés à **15** pour Earn, **15** pour Leverage Earn, **1** pour Surge, **2** pour RCH Surge et **2** pour Automator.

2025/02/18 8:00 UTC - 2025/04/07 8:00 UTC, ils sont fixés à **min(jours avant expiration, 20)** pour Earn, **min(jours avant expiration, 20)** pour Leverage Earn, **1** pour Surge, **2** pour RCH Surge et **abs(0.5 - takerCollateral / (takerCollateral + makerCollateral)) / 0.5 * 4** pour Automator.

Après cela, ils sont fixés à **min(jours avant expiration, 20)** pour Earn, **min(jours avant expiration, 20)** pour Leverage Earn, **1** pour Surge, **2** pour RCH Surge, **abs(0.5 - takerCollateral / (takerCollateral + makerCollateral)) / 0.5 * 2** pour Automator et **2** pour Dual.

**15%** de l'airdrop Automator sera facturé au RCH Deployer à des fins de brûlage de RCH. Enfin, un facteur d'ajustement universel final de **95%** est appliqué pour tenir compte de la réduction de l'airdrop payée au [dApp broker](../INTRO.md).

À mesure que SOFA se développe et que des protocoles supplémentaires arrivent, vous pouvez apprécier à quel point cette variable sera importante pour stimuler le TVL, et cela va être l'un des paramètres vitaux qui peuvent être votés par nos détenteurs de jetons de gouvernance $SOFA.

_Remarque : Le système capture des instantanés quotidiens. Les jetons $RCH sont disponibles à la réclamation chaque jour à 08:00 UTC après l'exécution de votre transaction._

### SOFA Earn

- SOFA Earn bénéficie du même poids d'airdrop que les autres produits Earn.

- De plus, tous les RCH déposés dans SOFA Earn recevront une récompense de dépôt de 5% APR.

- Chaque transaction dans le Vault associé (Mint/Burn/Harvest) déclenche le règlement des intérêts, entraînant des récompenses composées, de sorte que le rendement dépasse 5%.

- Les récompenses de SOFA Earn seront distribuées quotidiennement par le biais d'airdrops.

## 🤝 **Rencontrez $SOFA : Le Jeton de Gouvernance**

Le **jeton $SOFA est le jeton de gouvernance de l'écosystème SOFA.org**. En tant qu'organisation technologique décentralisée, à but non lucratif et open-source, toutes les décisions au sein de l'écosystème SOFA.org sont déterminées par les votes des détenteurs de jetons $SOFA. **En tant que jeton de gouvernance pur, $SOFA ne participe à aucun partage de bénéfices au sein de l'écosystème.**

## 🎤 La Voix de la Communauté

**Sujets Relatifs aux Votes de Gouvernance :**

1. Types de produits financiers à intégrer
2. Détermination du panier éligible de garanties prises en charge (USDT, etc.)
3. Admission de nouveaux protocoles partenaires dans l'écosystème SOFA.org
4. Répartition % des airdrops quotidiens de $RCH entre les différents produits intégrés
5. Répartition % des airdrops quotidiens de $RCH parmi les protocoles de l'écosystème
6. Rythme des airdrops de $RCH
7. Et d'autres décisions à venir à mesure que l'écosystème mûrit